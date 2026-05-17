<footer class="safe-area bg-brand">
    <div class="footer-content">
        <h2 class="footer-title">
            HAI VISTO COSA SO FARE.<br />
            ORA VEDIAMO COSA POSSIAMO CREARE INSIEME.
        </h2>

        <div class="footer-bottom">
            <div class="links-container">
                <a href="mailto:richipizzi04@gmail.com" class="footer-link">
                    <span class="link-label">EMAIL</span>
                    <span class="link-text">richipizzi04@gmail.com</span>
                </a>
                <a
                    href="https://instagram.com/richi_pizzi"
                    target="_blank"
                    class="footer-link"
                >
                    <span class="link-label">INSTAGRAM</span>
                    <span class="link-text">@richi_pizzi</span>
                </a>
                <a
                    href="https://tiktok.com/@richipizzi"
                    target="_blank"
                    class="footer-link"
                >
                    <span class="link-label">TIKTOK</span>
                    <span class="link-text">@richipizzi</span>
                </a>
            </div>

            <div class="footer-credits">
                <p>
                    &copy; {new Date().getFullYear()} Riccardo Pizzigoni. All rights
                    reserved.
                </p>
                <a
                    href="#"
                    class="back-to-top"
                    onclick={(e) => {
                        e.preventDefault();
                        window.scrollTo({ top: 0, behavior: "smooth" });
                    }}>Torna su &uarr;</a
                >
            </div>
        </div>

        <!-- Sfondo decorativo -->
        <svg
            class="footer-bg-star"
            viewBox="0 0 24 24"
            fill="var(--color-white)"
            xmlns="http://www.w3.org/2000/svg"
            ><path
                d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"
            /></svg
        >
    </div>
</footer>

<style>
    .bg-brand {
        background-color: var(--color-brand);
        color: var(--color-white);
    }

    footer {
        padding-top: clamp(4rem, 8vw, 6rem);
        padding-bottom: clamp(2rem, 5vw, 3rem);
        position: relative;
        overflow: hidden;
    }

    .footer-content {
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        position: relative;
        z-index: 1;
    }

    .footer-title {
        font-family: var(--font-sans);
        font-size: clamp(2rem, 3.5vw, 3.5rem);
        line-height: 1.2;
        font-weight: 800;
        margin-bottom: clamp(2rem, 4vw, 3rem);
        text-transform: uppercase;
        max-width: 30ch;
    }

    .footer-bottom {
        display: flex;
        flex-direction: column;
        gap: 3rem;
        margin-top: auto;
    }

    .links-container {
        display: flex;
        flex-direction: column;
        gap: 1.5rem;
    }

    .footer-link {
        display: flex;
        align-items: center;
        text-decoration: none;
        color: var(--color-white);
        transition:
            color 0.4s ease,
            transform 0.4s var(--ease-out-quart);
        padding-bottom: 1.5rem;
        border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        gap: 2rem;
    }

    .footer-link:hover {
        transform: translateX(10px);
        border-bottom-color: var(--color-white);
    }

    .link-label {
        font-family: var(--font-sans);
        font-size: 0.9rem;
        font-weight: 700;
        width: 100px;
        flex-shrink: 0;
        letter-spacing: 1px;
    }

    .link-text {
        font-family: var(--font-sans);
        font-size: clamp(1.2rem, 2vw, 2rem);
        font-weight: 500;
    }

    .footer-credits {
        display: flex;
        flex-direction: column;
        gap: 1rem;
        padding-top: 1.5rem;
        font-family: var(--font-sans);
        font-size: 0.9rem;
        color: rgba(255, 255, 255, 0.8);
    }

    .back-to-top {
        color: var(--color-white);
        text-decoration: none;
        transition: opacity 0.3s ease;
        text-transform: uppercase;
        font-weight: 600;
        letter-spacing: 1px;
    }

    .back-to-top:hover {
        opacity: 0.7;
    }

    .footer-bg-star {
        position: absolute;
        top: 10%;
        right: -5%;
        width: clamp(150px, 30vw, 350px);
        height: clamp(150px, 30vw, 350px);
        opacity: 0.05;
        pointer-events: none;
        z-index: -1;
        transform: rotate(15deg);
    }

    @media (min-width: 768px) {
        .links-container {
            flex-direction: row;
            justify-content: space-between;
            border-bottom: none;
        }

        .footer-link {
            flex-direction: column;
            align-items: flex-start;
            border-bottom: none;
            padding-bottom: 0;
            gap: 1rem;
        }

        .footer-link:hover {
            transform: translateY(-8px);
            border-bottom-color: transparent;
        }

        .footer-credits {
            flex-direction: row;
            justify-content: space-between;
            align-items: center;
            border-top: 1px solid rgba(255, 255, 255, 0.2);
        }
    }
</style>
