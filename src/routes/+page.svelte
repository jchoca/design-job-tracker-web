<script>
    import { createClient } from '@supabase/supabase-js';
    import { onMount } from 'svelte';

    const client = createClient("https://ihwemdtqgfiysjcujgin.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Imlod2VtZHRxZ2ZpeXNqY3VqZ2luIiwicm9sZSI6ImFub24iLCJpYXQiOjE2NzUxMjI0MjUsImV4cCI6MTk5MDY5ODQyNX0.O10ABGTqJVE46bgctle83bJmsjwkvEvimiPTVObArpE");

    let filterCriteria = "";
    let statusFilter = "";
    let projects = [];
    let projectsOnMount = [];
    let loaded = false;

    onMount(async () => {
        // todo: handle errors
        projects = (await client
        .from("projects")
        .select(`
            *,
            clients (
                name
            )
        `)).data;
        projectsOnMount = [...projects];
        loaded = true;
    })

    function filterByCriteria() {
        if (!loaded) {
            return;
        }

        if (!filterCriteria && !statusFilter) {
            projects = [...projectsOnMount];
            return;
        }

        if (filterCriteria) {
            projects = projectsOnMount.filter(proj => {
                let criteriaLower = filterCriteria.toLowerCase();
                return proj.job_prefix.toString().includes(criteriaLower) ||
                    proj.job_suffix.toLowerCase().includes(criteriaLower) ||
                    proj.name.toLowerCase().includes(criteriaLower) ||
                    proj.clients.name.toLowerCase().includes(criteriaLower);
            })
        }

        if (statusFilter) {
            projects = projectsOnMount.filter(proj => {
                return proj.status === statusFilter;
            });
        }
    }

    function formatDate(dateString) {
        if (!dateString) {
            return "N/A";
        }
        let parsed = new Date(dateString);
        return parsed.toLocaleDateString();
    }

    function formatStatus(status) {
        switch (status) {
            case "NotStarted":
                return "Not started";
            case "Started":
                return status;
            default:
                return "Unknown";
        }
    }
</script>

<div class="montserrat">
    <h1>Recent projects</h1>
    <div id="recent-projects">
        <div id="filters">
            <input bind:value={filterCriteria}
               on:input={filterByCriteria}
               aria-label="Filter by name or client"
               id="filter-box"
               class="filter-box"
               type="text"
               placeholder="Filter by name or client..." />
            <div id="filter-select-status-container">
                <label for="filter-select-status">Status</label>
                <select id="filter-select-status"
                        bind:value={statusFilter}
                        on:change={filterByCriteria}>
                    <option value="" selected></option>
                    <option value="NotStarted">Not started</option>
                    <option value="Started">Started</option>
                </select>
            </div>
        </div>
        {#if loaded}
            <div class="header-row">
                <div class="header">Name</div>
                <div class="header">Client</div>
                <div class="header">Status</div>
                <div class="header">My due date</div>
                <div class="header">Due date</div>
            </div>
            {#each projects as project}
                <div class="row">
                    <div>{project.job_prefix}-{project.job_suffix} {project.name}</div>
                    <div>{project.clients.name}</div>
                    <div>{formatStatus(project.status)}</div>
                    <div>{formatDate(project.internal_due_date)}</div>
                    <div>{formatDate(project.client_due_date)}</div>
                </div>
            {/each}
        {:else}
            <div style="text-align: center">Loading...</div>
        {/if}
    </div>
</div>

<style>
    .row, .header-row {
        border-bottom-width: 1px;
        border-bottom-color: rgb(0, 0, 0, 0.2);
        border-bottom-style: solid;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    #recent-projects {
        display: flex;
        flex-direction: column;
    }

    .row div, .header-row div {
        flex-grow: 1;
        width: 20%;
    }

    .header-row {
        font-weight: 500;
        padding-bottom: 5px;
    }

    .row {
        padding: 10px 0;
    }

    .header {
        cursor: pointer;
    }

    .filter-box {
        width: 50%;
    }

    #filters {
        display: flex;
        align-items: center;
        margin-bottom: 25px;
    }

    #filter-select-status-container {
        margin-left: 30px;
        font-weight: 500;
    }

    #filter-select-status-container select {
        margin-left: 5px;
    }
</style>