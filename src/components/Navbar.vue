<template>
  <div class="navbar-container">
    <ul>
        <router-link to="/"><li class="nav-item hoverable">Home</li></router-link>
        <router-link to="/"><li class="nav-item hoverable">About</li></router-link>
        <router-link to="/"><li class="nav-item hoverable">Services</li></router-link>
        <router-link to="/"><li class="nav-item hoverable">Social Media</li></router-link>
        <router-link to="/"><li class="nav-item hoverable">Case Studies</li></router-link>
        <router-link to="/"><li class="nav-item hoverable">Contact</li></router-link>
    </ul>
  </div>
</template>

<script>
export default {
    name: 'Navbar',

}
</script>

<style lang="scss" scoped>
@import '../assets/styles/global';

.navbar-container {
    //background: rgba(orange, 0.2);
    color: white;
    height: 120px;
    width: 100%;
    position: absolute;
    z-index: 999;
    padding-right: 64px;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    line-height: 0px;

    ul {
        list-style: none;
        display: flex;
        margin: 0px;
        padding-right: 64px;
    }

    a {
        text-decoration: none;
        color: white;
    }

    li {
        padding-right: 16px;
        padding-left: 16px;
        height: 0px;

        &:hover {
            opacity: 0.5;
            background: rgba(black, 0.5);
        }
    }
}

</style>