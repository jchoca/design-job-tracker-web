<script>
    let menuVisible = false;

    function toggleMenu() {
        menuVisible = !menuVisible;
    }
</script>

<div class="nav-container montserrat">
    <div>
        <div id="logo">
            Q
        </div>
        <div class="nav-item">
            <a href="/">Home</a>
        </div>
        <div class="nav-item">
            <a href="/clients">Clients</a>
        </div>
        <div class="nav-item">
            <a href="/projects">Projects</a>
        </div>
        <div class="nav-item">
            <a href="/timesheets">Timesheets</a>
        </div>
    </div>
    <div id="user-profile">
        <button class="button-no-styling" type="button" on:click={toggleMenu} on:onkeydown={toggleMenu} tabindex="0">
            <span id="user-profile-icon"
              class="material-icons"
              class:active={menuVisible}>person</span>
        </button>

        {#if menuVisible}
            <div id="user-profile-menu">
                <a href="/settings">
                    <div class="menu-item">
                        Settings
                    </div>
                </a>
                <a href="/logout">
                    <div class="menu-item">
                        Logout
                        &nbsp <span id="logout-icon" class="material-icons">logout</span>
                    </div>
                </a>
            </div>
        {/if}
    </div>
</div>
<div id="slot">
    <div>
        <slot></slot>
    </div>
</div>

<style>
    * {
        box-sizing: border-box;
    }

    @font-face {
        font-family: "Montserrat";
        src: url("/fonts/Montserrat-VariableFont_wght.ttf");
        font-weight: 125 950;
        font-stretch: 75% 125%;
        font-style: normal;
    }

    a, #logout-icon {
        color: rgb(255, 255, 255, 0.7);
        text-decoration: none;
    }

    .nav-container {
        display: flex;
        background-color: rgb(16, 64, 59);
        justify-content: space-between;
    }

    .nav-container div {
        display: flex;
        align-items: center;
    }

    .nav-item {
        padding: 10px;
        margin: 0 15px;
        border-color: #10403B;
        border-style: solid;
        border-width: 0 0 2px 0;
        font-weight: 500;
        -webkit-box-sizing: border-box;
        -moz-box-sizing: border-box;
        box-sizing: border-box;
        border-radius: 5px;
    }

    .nav-item:hover {
        background-color: rgb(255, 255, 255, 0.1);
        color: rgb(255, 255, 255, 0.8);
        cursor: pointer;
    }

    #slot {
        justify-content: center;
        display: flex;
    }

    #slot div {
        flex-grow: 1;
        margin: 0 40px;
    }

    #logo {
        color: white;
        font-size: 40px;
        margin: 10px 20px;
    }

    #user-profile {
        margin: 10px 20px;
        color: rgb(255, 255, 255, 0.8);
        cursor: pointer;
    }

    #user-profile-icon {
        font-size: 40px;
    }

    #user-profile-icon.active {
        font-weight: bold;
        color: rgb(255, 255, 255);
    }

    #user-profile-menu {
        position: absolute;
        right: 25px;
        top: 60px;
        background: rgb(138, 166, 163);
        display: flex;
        flex-direction: column;
        -webkit-box-shadow: 0px 0px 7px 0px rgba(0,0,0,0.75);
        -moz-box-shadow: 0px 0px 7px 0px rgba(0,0,0,0.75);
        box-shadow: 0px 0px 7px 0px rgba(0,0,0,0.75);
        color: rgb(0, 0, 0, 0.6);
        font-weight: 500;
    }

    #user-profile-menu a {
        width: 100%;
    }

    #user-profile-menu .menu-item {
        padding: 15px;
        width: 100%;
        justify-content: center;
        display: flex;
    }

    #user-profile-menu div:hover {
        background: rgb(16, 64, 59);
        color: rgb(255, 255, 255)
    }

    .button-no-styling {
        border: none;
        background: none;
        color: inherit;
        cursor: pointer;
    }

    :global(.montserrat) {
        font-family: "Montserrat", Verdana, sans-serif;
    }
</style>