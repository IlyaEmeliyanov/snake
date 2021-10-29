<!-- Script -->
<script>
    const logoImgSrc = "";
</script>

<!-- HTML -->
<header class="header">
    <div class="logo">
        <img src={logoImgSrc || "🐍"} alt="logo-icon">
        <h2>Snaky</h2>
    </div>
</header>

<!-- Styles -->
<style>
    .header {
        display: flex;
        justify-content: space-between;
        padding: 2rem 3rem;
    }

    .logo {
        display: flex;
        justify-content: space-evenly;
        align-items: center;
    }

</style>