<script>
  export let segment;
</script>

<style>
  nav {
    border-bottom: 1px solid rgba(255, 62, 0, 0.1);
    font-weight: 300;
    padding: 0 1em;
    background-color: #17819B;
  }

  ul {
    margin: 0;
    padding: 0;
    background-color: #17819B;
  }

  /* clearfix */
  ul::after {
    content: "";
    display: block;
    clear: both;
  }

  li {
    display: block;
    float: left;
    background-color: #17819B;
    color: #F8F0EE;
  }

  .selected {
    position: relative;
    display: inline-block;

  }

  .selected::after {
    position: absolute;
    content: "";
    width: calc(100% - 1em);
    height: 2px;
    background-color: rgb(255, 62, 0);
    display: block;
    bottom: -1px;
  }

  a {
    text-decoration: none;
    padding: 1em 0.5em;
    display: block;
    background-color: #17819B;
    color: #F8F0EE;
  }
</style>

<nav>
  <ul>
    <li>
      <a class='{segment === undefined ? "selected" : ""}' href=".">home</a>
    </li>

    <li>
      <a class='{segment === "alumnos" ? "selected" : ""}' href="Alumnos">alumnos</a>
    </li>

    <li>
      <a class='{segment === "gráfica" ? "selected" : ""}' href="Chart">gráfica</a>
    </li>
  </ul>
</nav>
