<script>
    import favicon from "$lib/assets/favicon.svg";

    import "$lib/styles/reset.css";
    import "$lib/styles/tokens.css";
    import "$lib/styles/base.css";
    import "$lib/styles/utilities.css";

    import Header from "$lib/components/Header.svelte";
    import Footer from "$lib/components/Footer.svelte";

    let { children } = $props();
</script>

<svelte:head>
    <link rel="icon" href={favicon} />

    <link rel="stylesheet" href="https://use.typekit.net/cmj3ghn.css">

    <link href="https://db.onlinewebfonts.com/c/92649d5606272c17cb8021b160c2460e?family=Edwardian+Script+ITC+Regular" rel="stylesheet">

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous">
    <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
</svelte:head>

<Header />

<main>
    {@render children()}
</main>

<Footer />