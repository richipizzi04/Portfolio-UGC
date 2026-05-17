<header class="safe-area">
    <a
        href="https://www.instagram.com/richi_pizzi?igsh=MTkwMzY5anM2MzhkZA%3D%3D&utm_source=qr"
        target="_blank"
        class="logo">RICCARDO PIZZIGONI</a
    >
    <nav>
        <a href="#ugc" class="nav-link">UGC</a>
        <span class="separator">|</span>
        <a href="#content-creator" class="nav-link">CONTENT CREATOR</a>
    </nav>
</header>

<style>
    header {
        padding-block: var(--size-4);
        display: flex;
        justify-content: space-between;
        align-items: center;
        text-transform: uppercase;
        letter-spacing: 0.05em;
        position: sticky;
        top: 0;
        z-index: 9000;
        background-color: var(--color-bg, #ffffff);
    }

    nav {
        display: flex;
        gap: var(--size-4);
        align-items: center;
    }

    .separator {
        opacity: 0.3;
    }

    .logo {
        font-size: var(--size-5); /* Dimensione base per Desktop */
        color: inherit;
        text-decoration: none;
        transition: color 0.3s ease;
    }

    .logo:hover {
        color: var(--color-brand);
    }

    .nav-link {
        color: inherit;
        text-decoration: none;
        transition: color 0.3s ease;
        cursor: pointer;
    }

    .nav-link:hover {
        color: var(--color-brand);
    }

    .nav-link:active {
        color: var(--color-brand);
    }

    /* ========================================= */
    /* === MODIFICHE MOBILE (max 768px) === */ /* --- MODIFICA user: tutte le modifiche ingrandite incluse quicite: --- */
    /* ========================================= */
    @media (max-width: 768px) {
        header {
            padding-top: 1.5rem;
            flex-direction: column;
            gap: 0.5rem;
            text-align: center;
        }

        .logo {
            font-size: 1.1rem;
        }

        nav {
            justify-content: center;
            width: 100%;
            font-size: 0.9rem;
        }
    }
</style>
