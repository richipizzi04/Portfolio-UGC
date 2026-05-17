<script>
    import { onMount } from "svelte";
    import gsap from "gsap";
    import { ScrollTrigger } from "gsap/dist/ScrollTrigger";

    /* --- LOGICA MODALE VIDEO --- */
    let isModalOpen = $state(false);
    let currentVideoSrc = $state("");

    function openModal(src) {
        currentVideoSrc = src;
        isModalOpen = true;
        document.body.style.overflow = "hidden"; // Blocca lo scroll del sito dietro
    }

    function closeModal() {
        isModalOpen = false;
        currentVideoSrc = "";
        document.body.style.overflow = ""; // Sblocca lo scroll
    }

    function handleVideoClick(e, src) {
        if (window.innerWidth <= 768) {
            openModal(src);
        } else {
            // Comportamento Desktop: play/pause inline
            const phone = e.currentTarget;
            const video = phone.querySelector("video");
            const overlay = phone.querySelector(".play-overlay");
            if (video) {
                if (video.paused) {
                    video.play();
                    if (overlay) overlay.classList.add("is-playing");
                } else {
                    video.pause();
                    if (overlay) overlay.classList.remove("is-playing");
                }
            }
        }
    }
    /* ----------------------------- */

    /* In futuro qui importeremo i video dalla cartella static/videos */

    let introScreen;
    let introText1;
    let introText2;

    let letterV;
    let letterC;
    let wordRest1;
    let wordRest2;
    let heroImage;
    let heroStar;
    let tagElements = [];
    let dashes = [];

    const tags = ["DESIGN", "FASHION", "LIFESTYLE", "ART"];

    onMount(() => {
        gsap.registerPlugin(ScrollTrigger);
        const tl = gsap.timeline();

        gsap.set([".marquee", ".about", ".collaborations", ".reasons"], {
            autoAlpha: 0,
        });

        window.scrollTo(0, 0);
        setTimeout(() => window.scrollTo(0, 0), 10);
        document.body.style.overflow = "hidden";

        // 1. Animazione Intro Screen
        tl.fromTo(
            introText1,
            { opacity: 0, filter: "blur(15px)", scale: 1.05, y: 15 },
            {
                opacity: 1,
                filter: "blur(0px)",
                scale: 1,
                y: 0,
                duration: 0.8,
                ease: "power3.out",
                delay: 0.2,
            },
        )
            .to(introText1, {
                opacity: 0,
                filter: "blur(10px)",
                scale: 0.95,
                y: -15,
                duration: 0.5,
                ease: "power2.in",
                delay: 1.0,
            })
            .fromTo(
                introText2,
                { opacity: 0, filter: "blur(15px)", scale: 1.05, y: 15 },
                {
                    opacity: 1,
                    filter: "blur(0px)",
                    scale: 1,
                    y: 0,
                    duration: 0.8,
                    ease: "power3.out",
                    delay: 0.1,
                },
            )
            .to(introText2, {
                opacity: 0,
                filter: "blur(10px)",
                scale: 0.95,
                y: -15,
                duration: 0.5,
                ease: "power2.in",
                delay: 1.0,
            })
            .to(
                introScreen,
                {
                    yPercent: -100,
                    duration: 0.8,
                    ease: "expo.inOut",
                    onComplete: () => {
                        document.body.style.overflow = "";
                        introScreen.style.display = "none";
                    },
                },
                "-=0.1",
            );

        // --- SEQUENZA HOMEPAGE ---
        tl.fromTo(
            [letterV, letterC],
            { clipPath: "inset(-100% 100% -100% -100%)", opacity: 0 },
            {
                clipPath: "inset(-100% -100% -100% -100%)",
                opacity: 1,
                duration: 1.2,
                ease: "power2.inOut",
                stagger: 0.2,
                clearProps: "clipPath",
            },
            "-=0.2",
        );

        tl.fromTo(
            [wordRest1, wordRest2],
            { opacity: 0, x: -20, filter: "blur(5px)" },
            {
                opacity: 1,
                x: 0,
                filter: "blur(0px)",
                duration: 0.8,
                ease: "power3.out",
                stagger: 0.2,
                clearProps: "filter",
            },
            "-=1.0",
        );

        tl.fromTo(
            heroImage,
            { opacity: 0, x: 300 },
            { opacity: 1, x: 0, duration: 1, ease: "power3.out" },
            "-=0.4",
        );

        tl.fromTo(
            heroStar,
            { opacity: 0, scale: 0, rotate: -90 },
            {
                opacity: 1,
                scale: 1,
                rotate: 0,
                duration: 0.8,
                ease: "elastic.out(1, 0.6)",
            },
            "-=0.5",
        );

        const letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ!@#$%&";
        tl.fromTo(
            dashes,
            { opacity: 0, scale: 0 },
            {
                opacity: 0.3,
                scale: 1,
                duration: 0.5,
                stagger: 0.1,
                ease: "back.out(2)",
            },
            "-=0.2",
        );

        tagElements.forEach((el, index) => {
            const finalWord = tags[index];
            const dummyObj = { value: 0 };
            tl.fromTo(
                el,
                { opacity: 0 },
                { opacity: 1, duration: 0.1 },
                "-=0.3",
            );
            tl.to(
                dummyObj,
                {
                    value: finalWord.length,
                    duration: 0.8,
                    ease: "none",
                    onUpdate: () => {
                        const progress = Math.floor(dummyObj.value);
                        el.innerText = finalWord
                            .split("")
                            .map((char, i) =>
                                i < progress
                                    ? finalWord[i]
                                    : letters[
                                          Math.floor(
                                              Math.random() * letters.length,
                                          )
                                      ],
                            )
                            .join("");
                    },
                },
                `<+${index * 0.15}`,
            );
        });

        tl.to(
            [".marquee", ".about"],
            {
                autoAlpha: 1,
                duration: 1,
                ease: "power2.out",
            },
            "-=4",
        );

        // 2. Il resto del sito compare regolarmente
        tl.to(
            [".collaborations", ".reasons"],
            {
                autoAlpha: 1,
                duration: 1,
                ease: "power2.out",
            },
            "-=0.5",
        );

        // --- SCROLLYTELLING BASE ---
        gsap.from(".marquee-content h2", {
            scrollTrigger: { trigger: ".marquee", start: "top 85%" },
            y: 50,
            opacity: 0,
            duration: 0.8,
            ease: "back.out(1.5)",
        });
        gsap.from(".about-left", {
            scrollTrigger: { trigger: ".about", start: "top 75%" },
            x: -50,
            opacity: 0,
            duration: 1,
            ease: "power3.out",
            clearProps: "all",
        });
        gsap.from(".about-right p", {
            scrollTrigger: {
                trigger: ".about",
                start: "top 80%",
                end: "center 50%",
                scrub: 1,
            },
            y: 30,
            opacity: 0,
            duration: 0.8,
            stagger: 0.2,
            ease: "power2.out",
            clearProps: "all",
        });

        // --- NUOVA ANIMAZIONE TELEFONI CHE COMPAIONO DA SOTTO ---
        const mmPhones = gsap.matchMedia();

        gsap.utils.toArray(".project-slide").forEach((slide) => {
            const phones = slide.querySelectorAll(".phone-mockup");

            // Impostiamo una posizione sfalsata e ruotata a "ventaglio" solo su Desktop
            mmPhones.add("(min-width: 769px)", () => {
                if (phones[0]) gsap.set(phones[0], { y: 15, rotation: -1.5 });
                if (phones[1])
                    gsap.set(phones[1], { y: -15, rotation: 0, zIndex: 2 });
                if (phones[2]) gsap.set(phones[2], { y: 15, rotation: 1.5 });
            });

            // Su mobile azzeriamo per allinearli
            mmPhones.add("(max-width: 768px)", () => {
                phones.forEach((phone) =>
                    gsap.set(phone, { y: 0, rotation: 0 }),
                );
            });

            // I telefoni emergono dal basso
            gsap.from(phones, {
                scrollTrigger: {
                    trigger: slide,
                    start: "top 75%",
                },
                y: "+=250",
                opacity: 0,
                duration: 1.2,
                stagger: 0.15,
                ease: "back.out(1.2)",
                onComplete: () => {
                    // Floating animation continua
                    phones.forEach((phone, i) => {
                        gsap.to(phone, {
                            y: i === 1 ? "-=15" : "+=15",
                            rotation: i === 1 ? "+=2" : "-=2",
                            duration: 2.5 + i * 0.2,
                            yoyo: true,
                            repeat: -1,
                            ease: "sine.inOut",
                        });
                    });
                },
            });

            // Hover interactions
            phones.forEach((phone, i) => {
                phone.addEventListener("mouseenter", () => {
                    gsap.to(phone, {
                        scale: 1.05,
                        boxShadow:
                            "0 0 0 2px #d1d1d1, 0 0 0 4px #a3a3a3, 15px 30px 50px rgba(0,0,0,0.3)",
                        duration: 0.4,
                        ease: "back.out(2)",
                        overwrite: "auto",
                    });
                });
                phone.addEventListener("mouseleave", () => {
                    gsap.to(phone, {
                        scale: 1,
                        boxShadow:
                            "0 0 0 2px #d1d1d1, 0 0 0 4px #a3a3a3, 10px 20px 40px rgba(0,0,0,0.15)",
                        duration: 0.4,
                        ease: "power2.out",
                        overwrite: "auto",
                    });
                });

                // NOTA: Il vecchio evento "click" per play/pause è stato rimosso per far funzionare il Modale Svelte.
            });
        });

        // Animazione della stella principale "Collaborazioni"
        gsap.from(".collab-main-star", {
            scrollTrigger: {
                trigger: ".collaborations",
                start: "top 15%",
                end: "top -45%",
                scrub: 1,
            },
            x: "100vw",
            rotation: 720,
            ease: "power2.out",
        });

        // I testi compaiono in cascata quando la loro slide entra nello schermo
        gsap.utils.toArray(".project-slide").forEach((slide) => {
            const infoElements = slide.querySelectorAll(
                ".project-info h3, .project-info li",
            );
            gsap.from(infoElements, {
                scrollTrigger: {
                    trigger: slide,
                    start: "top 75%",
                },
                opacity: 0,
                x: 30,
                duration: 0.8,
                stagger: 0.15,
                ease: "power2.out",
            });

            // Aggiungiamo un sottile effetto parallax ai testi
            const infoContainer = slide.querySelector(".project-info");
            if (infoContainer) {
                gsap.to(infoContainer, {
                    scrollTrigger: {
                        trigger: slide,
                        start: "top 85%",
                        end: "bottom 15%",
                        scrub: 1,
                    },
                    y: -40,
                    ease: "none",
                });
            }
        });

        gsap.from(".reasons .section-title-reasons", {
            scrollTrigger: {
                trigger: ".reasons",
                start: "top 80%",
            },
            y: 30,
            opacity: 0,
            duration: 0.8,
            ease: "power2.out",
        });
        gsap.from(".reason", {
            scrollTrigger: {
                trigger: ".reasons-list",
                start: "top 75%",
            },
            y: 40,
            opacity: 0,
            duration: 0.8,
            stagger: 0.2,
            ease: "power2.out",
        });

        gsap.to(".reasons-bg-star", {
            scrollTrigger: {
                trigger: ".reasons",
                start: "top bottom",
                end: "bottom top",
                scrub: 1,
            },
            rotation: 180,
            y: 150,
            ease: "none",
        });

        // --- ANIMAZIONE NUMERI INSIGHTS ---
        gsap.utils.toArray(".num-animate").forEach((el) => {
            const target = parseFloat(el.getAttribute("data-target"));
            const prefix = el.getAttribute("data-prefix") || "";
            const suffix = el.getAttribute("data-suffix") || "";
            const isFloat = el.getAttribute("data-target").includes(".");

            let dummy = { val: 0 };

            gsap.to(dummy, {
                scrollTrigger: {
                    trigger: el,
                    start: "top 75%",
                },
                val: target,
                duration: 4,
                ease: "power3.out",
                onUpdate: () => {
                    let formatted = isFloat
                        ? dummy.val.toFixed(1)
                        : Math.round(dummy.val);
                    el.innerHTML = prefix + formatted + suffix;
                },
            });
        });

        // Assicuriamoci che il grafico parta vuoto (offset = 188.5)
        gsap.set(".donut-fill", { strokeDashoffset: 188.5 });

        gsap.to(".donut-fill", {
            scrollTrigger: {
                trigger: ".donut-chart-container",
                start: "top 75%",
            },
            strokeDashoffset: 188.5 - 188.5 * 0.595, // 59.5%
            duration: 4,
            ease: "power3.out",
        });

        gsap.utils.toArray(".insight-block").forEach((block, i) => {
            gsap.from(block, {
                scrollTrigger: {
                    trigger: block,
                    start: "top 75%",
                },
                y: 50,
                opacity: 0,
                duration: 1.5,
                delay: i * 0.3, // Sfalsamento manuale invece di stagger perché i trigger sono separati
                ease: "power3.out",
            });
        });

        // --- ANIMAZIONE STELLA CHE VIAGGIA E SEZIONE ROSSA CON MATCHMEDIA ---
        let journeyTl;
        const createStarAnim = () => {
            if (journeyTl) journeyTl.kill();
            ScrollTrigger.refresh();

            // Creiamo il MatchMedia per differenziare le animazioni Mobile vs Desktop ---
            let mm = gsap.matchMedia();

            // 1. La stella iniziale ruota semplicemente su se stessa
            gsap.to(heroStar, {
                scrollTrigger: {
                    trigger: "body",
                    start: "top top",
                    end: "bottom bottom",
                    scrub: 1,
                },
                rotation: 1440, // Aumentata la rotazione per fare molti più giri durante lo scroll
                ease: "none",
            });

            // 2. La stella grande della sezione rossa ---
            // DESKTOP: entra da sinistra ed esce a destra con pin
            mm.add("(min-width: 769px)", () => {
                gsap.set(".bg-star", {
                    x: "-50vw",
                    opacity: 1,
                    rotation: 0,
                    left: 0,
                    margin: 0,
                });

                journeyTl = gsap.timeline({
                    scrollTrigger: {
                        trigger: ".brand-scroll-wrapper",
                        start: "top 5%",
                        end: "+=100%", // pinna per la durata dello scrolling
                        pin: true,
                        pinSpacing: true,
                        scrub: 1,
                    },
                });

                journeyTl.to(".bg-star", {
                    x: "110vw",
                    rotation: 360,
                    ease: "none",
                });
            });

            // MOBILE: sta ferma al centro e gira sul posto ---
            mm.add("(max-width: 768px)", () => {
                // Lasciamo che sia il CSS a gestire la posizione!
                gsap.set(".bg-star", {
                    x: 0,
                    opacity: 1,
                    rotation: 0,
                });

                journeyTl = gsap.timeline({
                    scrollTrigger: {
                        trigger: ".about", // Si attiva solo quando scendi sulla sezione About ---
                        start: "top bottom",
                        end: "bottom top",
                        scrub: 1,
                    },
                });

                journeyTl.to(".bg-star", {
                    rotation: 360, // Gira solo sul posto ---
                    ease: "none",
                });
            });
        };

        setTimeout(createStarAnim, 300);
        window.addEventListener("resize", createStarAnim);
    });
</script>

<div class="intro-screen" bind:this={introScreen}>
    <div class="intro-text" bind:this={introText1}>
        Le persone ignorano i brand noiosi.
    </div>
    <div class="intro-text" bind:this={introText2}>
        Iniziamo a farci notare.
    </div>
</div>

<section class="hero safe-area">
    <div class="hero-content">
        <div class="title-wrapper">
            <h1 class="main-title">
                <div class="word-line word-visual">
                    <span class="font-script letter-script" bind:this={letterV}
                        >V</span
                    ><span class="word-rest" bind:this={wordRest1}>isual</span>
                </div>
                <div class="word-line word-creator">
                    <span class="font-script letter-script" bind:this={letterC}
                        >C</span
                    ><span class="word-rest" bind:this={wordRest2}>reator</span>
                </div>
            </h1>
            <div class="hero-tags">
                {#each tags as tag, i}
                    <span class="tag-container">
                        <span class="tag-hidden">{tag}</span>
                        <span class="tag-scramble" bind:this={tagElements[i]}
                            >{tag}</span
                        >
                    </span>
                    {#if i < tags.length - 1}
                        <span class="dash" bind:this={dashes[i]}>-</span>
                    {/if}
                {/each}
            </div>
            <svg
                class="star"
                bind:this={heroStar}
                viewBox="0 0 24 24"
                fill="var(--color-brand)"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"
                />
            </svg>
        </div>
    </div>

    <div class="hero-image-wrapper">
        <img
            src="/images/copertina.png"
            alt="Riccardo Pizzigoni"
            class="hero-image"
            bind:this={heroImage}
        />
    </div>
</section>

<div
    class="brand-scroll-wrapper"
    style="position: relative; z-index: 5; margin-top: 0vh;"
>
    <section class="marquee bg-brand">
        <div class="marquee-content">
            <h2>LE PERSONE IGNORANO IL DESIGN CHE IGNORA LE PERSONE.</h2>
        </div>
    </section>

    <section class="about bg-brand safe-area">
        <div class="about-grid">
            <div class="about-left">
                <div class="star-text-wrapper">
                    <svg
                        class="bg-star"
                        viewBox="0 0 24 24"
                        fill="var(--color-white)"
                        xmlns="http://www.w3.org/2000/svg"
                    >
                        <path
                            d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"
                        />
                    </svg>
                    <h2 class="about-title-inside">
                        <span class="script-a">A</span>
                        <span class="neulis-rest">bout<br />me</span>
                    </h2>
                </div>
            </div>

            <div class="description about-right">
                <p>
                    La differenza tra un buon lavoro e un lavoro eccezionale sta
                    nei dettagli. Nei miei video fondo 2 anime: il rigore visivo
                    del design e la freschezza dei social. Risultato? Contenuti
                    autentici, curati al millimetro e guidati da uno
                    storytelling ironico che sa catturare l’attenzione dal primo
                    secondo.
                </p>
                <p class="right-align">
                    Il design non è solo estetica, è strategia.
                </p>
                <p>
                    Come studente di Design della Comunicazione al Politecnico
                    di Milano e Content Creator, non mi limito a registrare
                    video: progetto messaggi visivi.
                </p>
            </div>
        </div>
    </section>
</div>

<section class="collaborations safe-area">
    <div class="collab-header">
        <h2 class="collab-title">
            <span class="font-script letter-collab">C</span><span
                class="word-rest-collab">ollaborazioni</span
            >
            <svg
                class="collab-main-star"
                viewBox="0 0 24 24"
                fill="var(--color-brand)"
                xmlns="http://www.w3.org/2000/svg"
                ><path
                    d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"
                /></svg
            >
        </h2>
    </div>

    <div class="collab-vertical-list">
        <div class="project-slide" id="ugc" style="scroll-margin-top: 100px;">
            <div class="phones-group">
                <div
                    class="phone-mockup"
                    onclick={(e) =>
                        handleVideoClick(e, "/videos/fromboxtoart.mp4")}
                >
                    <div class="phone-notch"></div>
                    <div class="phone-screen">
                        <video
                            src="/videos/fromboxtoart.mp4#t=0.1"
                            loop
                            playsinline
                            class="mockup-video"
                        ></video>
                        <div class="play-overlay">
                            <svg
                                viewBox="0 0 24 24"
                                fill="white"
                                width="48"
                                height="48"><path d="M8 5v14l11-7z" /></svg
                            >
                        </div>
                    </div>
                </div>
                <div
                    class="phone-mockup"
                    onclick={(e) =>
                        handleVideoClick(e, "/videos/howitchanges.mp4")}
                >
                    <div class="phone-notch"></div>
                    <div class="phone-screen">
                        <video
                            src="/videos/howitchanges.mp4#t=0.1"
                            loop
                            playsinline
                            class="mockup-video"
                        ></video>
                        <div class="play-overlay">
                            <svg
                                viewBox="0 0 24 24"
                                fill="white"
                                width="48"
                                height="48"><path d="M8 5v14l11-7z" /></svg
                            >
                        </div>
                    </div>
                </div>
                <div
                    class="phone-mockup"
                    onclick={(e) => handleVideoClick(e, "/videos/vangogh.MP4")}
                >
                    <div class="phone-notch"></div>
                    <div class="phone-screen">
                        <video
                            src="/videos/vangogh.MP4#t=0.1"
                            loop
                            playsinline
                            class="mockup-video"
                        ></video>
                        <div class="play-overlay">
                            <svg
                                viewBox="0 0 24 24"
                                fill="white"
                                width="48"
                                height="48"><path d="M8 5v14l11-7z" /></svg
                            >
                        </div>
                    </div>
                </div>
            </div>
            <div class="project-info">
                <h3>LENOVO</h3>
                <ul>
                    <li>Campagna Lancio Yoga Tab</li>
                    <li>Obiettivo: mostrare le potenzialità del tablet</li>
                    <li>
                        Target: artisti, designer, studenti, tutti coloro che
                        usano il tablet per lavorare, creare o studiare
                    </li>
                    <li>novembre 2025</li>
                </ul>
            </div>
        </div>

        <div
            class="project-slide"
            id="content-creator"
            style="scroll-margin-top: 100px;"
        >
            <div class="phones-group">
                <div
                    class="phone-mockup"
                    onclick={(e) =>
                        handleVideoClick(e, "/videos/PALAZZO LITTA.mp4")}
                >
                    <div class="phone-notch"></div>
                    <div class="phone-screen">
                        <video
                            src="/videos/PALAZZO LITTA.mp4#t=0.1"
                            loop
                            playsinline
                            class="mockup-video"
                        ></video>
                        <div class="play-overlay">
                            <svg
                                viewBox="0 0 24 24"
                                fill="white"
                                width="48"
                                height="48"><path d="M8 5v14l11-7z" /></svg
                            >
                        </div>
                    </div>
                </div>
                <div
                    class="phone-mockup"
                    onclick={(e) =>
                        handleVideoClick(e, "/videos/QCxETHIMO.mp4")}
                >
                    <div class="phone-notch"></div>
                    <div class="phone-screen">
                        <video
                            src="/videos/QCxETHIMO.mp4#t=0.1"
                            loop
                            playsinline
                            class="mockup-video"
                        ></video>
                        <div class="play-overlay">
                            <svg
                                viewBox="0 0 24 24"
                                fill="white"
                                width="48"
                                height="48"><path d="M8 5v14l11-7z" /></svg
                            >
                        </div>
                    </div>
                </div>
                <div
                    class="phone-mockup"
                    onclick={(e) => handleVideoClick(e, "/videos/GUCCI.mp4")}
                >
                    <div class="phone-notch"></div>
                    <div class="phone-screen">
                        <video
                            src="/videos/GUCCI.mp4#t=0.1"
                            loop
                            playsinline
                            class="mockup-video"
                        ></video>
                        <div class="play-overlay">
                            <svg
                                viewBox="0 0 24 24"
                                fill="white"
                                width="48"
                                height="48"><path d="M8 5v14l11-7z" /></svg
                            >
                        </div>
                    </div>
                </div>
            </div>
            <div class="project-info">
                <h3>MILANO DESIGN WEEK 2026<br />IDNTT x DDN</h3>
                <ul>
                    <li>
                        Realizzare contenuti in showroom, installazioni per
                        brand
                    </li>
                    <li>
                        Obiettivo: aumentare la visibilità del mondo del design
                    </li>
                    <li>
                        Target: ragazzi che non conoscono nulla di design ma che
                        potrebbero essere interessati
                    </li>
                </ul>
            </div>
        </div>
    </div>
</section>

<section class="reasons bg-brand safe-area">
    <h2 class="section-title-reasons">
        <span class="font-script letter-script-reasons">P</span>erché lavorare
        con me <span class="font-script question-mark">?</span>
    </h2>
    <div class="reasons-container">
        <svg
            class="reasons-bg-star"
            viewBox="0 0 24 24"
            fill="var(--color-white)"
            xmlns="http://www.w3.org/2000/svg"
            ><path
                d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"
            /></svg
        >
        <div class="reasons-list">
            <div
                class="reason"
                onclick={(e) => e.currentTarget.classList.toggle("is-open")}
            >
                <h3>DESIGN STRATEGICO</h3>
                <div class="reason-content">
                    <p>
                        Progettazione del messaggio visivo basata sui principi
                        della comunicazione. Non solo estetica, ma progettazione
                        ed editing strutturato.
                    </p>
                </div>
            </div>
            <div
                class="reason"
                onclick={(e) => e.currentTarget.classList.toggle("is-open")}
            >
                <h3>ESTETICA PREMIUM</h3>
                <div class="reason-content">
                    <p>
                        Attenzione maniacale al dettaglio, al set e al lighting.
                        Contenuti di alta qualità pronti per il mercato del
                        lusso, del design, della quotidianità.
                    </p>
                </div>
            </div>
            <div
                class="reason"
                onclick={(e) => e.currentTarget.classList.toggle("is-open")}
            >
                <h3>AFFIDABILITÀ E PROFESSIONALITÀ</h3>
                <div class="reason-content">
                    <p>
                        Gestione professionale dei file, rispetto rigoroso delle
                        deadline e comunicazione trasparente in ogni fase del
                        progetto.
                    </p>
                </div>
            </div>
            <div
                class="reason"
                onclick={(e) => e.currentTarget.classList.toggle("is-open")}
            >
                <h3>STORYTELLING DINAMICO</h3>
                <div class="reason-content">
                    <p>
                        Narrazione fluida, ritmo social e un pizzico di ironia
                        per abbattere la barriera tra brand e utente,
                        massimizzando il tempo di visione.
                    </p>
                </div>
            </div>
        </div>
    </div>
</section>

<section class="insights safe-area">
    <h2 class="insights-title">
        <span class="font-script letter-script-insights">S</span><span
            class="word-rest-insights">ocial</span
        >
        &nbsp;&nbsp;
        <span class="font-script letter-script-insights">I</span><span
            class="word-rest-insights">nsights</span
        >
    </h2>

    <div class="insights-grid">
        <div class="insight-block">
            <h3>Instagram Analytics (ultimi 30gg)</h3>
            <div class="ig-data-grid">
                <div class="ig-col">
                    <div class="data-item">
                        <span class="data-label">Account Raggiunti (35K)</span>
                        <span
                            class="data-value num-animate"
                            data-target="976"
                            data-prefix="+"
                            data-suffix="%">+0%</span
                        >
                    </div>
                    <div class="data-item">
                        <span class="data-label">Visualizzazioni totali</span>
                        <span
                            class="data-value num-animate"
                            data-target="113.7"
                            data-suffix="K">0K</span
                        >
                    </div>
                    <div class="data-item">
                        <span class="data-label">Visite al profilo</span>
                        <span
                            class="data-value num-animate"
                            data-target="94"
                            data-prefix="+"
                            data-suffix="%">+0%</span
                        >
                    </div>
                </div>
                <div class="ig-col ig-col-right">
                    <div class="data-item">
                        <span class="data-label">Distribuzione pubblico</span>
                        <div class="donut-chart-container">
                            <svg viewBox="0 0 100 100" class="donut-svg">
                                <path
                                    d="M 22,78 A 40,40 0 1,1 78,78"
                                    fill="none"
                                    stroke="#f4f4f4"
                                    stroke-width="12"
                                    stroke-linecap="round"
                                />
                                <path
                                    class="donut-fill"
                                    d="M 22,78 A 40,40 0 1,1 78,78"
                                    fill="none"
                                    stroke="var(--color-brand)"
                                    stroke-width="12"
                                    stroke-linecap="round"
                                    stroke-dasharray="188.5"
                                    stroke-dashoffset="188.5"
                                />
                            </svg>
                            <span class="donut-label"
                                ><span
                                    class="num-animate"
                                    data-target="59.5"
                                    data-suffix="%">0%</span
                                > Non-Follower</span
                            >
                        </div>
                    </div>
                    <div class="data-item mt-auto">
                        <span class="data-label"
                            >Target Geografico Principale</span
                        >
                        <span class="data-value label-city"
                            >Milano <span class="text-small"
                                >(<span
                                    class="num-animate"
                                    data-target="7.5"
                                    data-suffix="%">0%</span
                                >)</span
                            ></span
                        >
                    </div>
                </div>
            </div>
        </div>

        <div class="insight-block">
            <h3>TikTok Insights (365gg)</h3>
            <div class="tk-data-grid">
                <div class="data-item">
                    <span class="data-label">Visualizzazioni totali</span>
                    <span
                        class="data-value num-animate"
                        data-target="56"
                        data-suffix="M">0M</span
                    >
                </div>
                <div class="data-item">
                    <span class="data-label">Traffico Per te</span>
                    <span
                        class="data-value num-animate"
                        data-target="88.5"
                        data-suffix="%">0%</span
                    >
                </div>
            </div>
        </div>
    </div>
</section>

{#if isModalOpen}
    <div class="video-modal" onclick={closeModal}>
        <button class="close-btn" onclick={closeModal}>✕</button>
        <video
            src={currentVideoSrc}
            controls
            autoplay
            playsinline
            class="fullscreen-video"
            onclick={(e) => e.stopPropagation()}
        >
        </video>
    </div>
{/if}

<style>
    :global(html),
    :global(body) {
        overflow-x: hidden !important;
        width: 100vw; /* Usa 100vw invece di 100% per maggiore sicurezza */
        max-width: 100%;
        position: relative;
    }

    .intro-screen {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background-color: var(--color-brand);
        z-index: 9999;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }

    .intro-text {
        position: absolute;
        font-family: var(--font-sans);
        font-weight: 600;
        font-size: clamp(1rem, 3.5vw, 2.5rem);
        color: var(--color-white, #ffffff);
        text-align: center;
        opacity: 0;
        padding: var(--size-4);
        max-width: 95vw;
        width: 100%;
        letter-spacing: -0.01em;
    }

    section {
        padding-block: var(--size-10);
    }
    .about {
        padding-top: 0;
        padding-bottom: 0;
    }

    .section-title {
        font-size: clamp(3rem, 6vw, 6rem);
        color: var(--color-brand);
        margin-bottom: var(--size-6);
    }

    .hero {
        position: relative;
        z-index: 10;
        min-height: 80vh;
        display: flex;
        align-items: center;
        overflow: visible;
    }

    .hero-content {
        position: relative;
        z-index: 2;
        margin-top: -5vh;
        width: 100%;
    }

    .title-wrapper {
        display: inline-flex;
        flex-direction: column;
    }

    .hero h1 {
        display: flex;
        flex-direction: column;
        color: var(--color-brand);
        margin: 0;
        line-height: 0.8;
        padding-top: 4vh;
        margin-left: -3vw;
    }

    .word-line {
        display: flex;
        align-items: baseline;
    }

    .word-creator {
        margin-left: 6vw;
        margin-top: -12vh;
    }

    .letter-script {
        font-size: clamp(8rem, 20vw, 22rem);
        padding-right: 0vw;
        position: relative;
        z-index: 100;
        display: inline-block;
    }

    .word-rest {
        font-family: var(--font-sans);
        font-size: clamp(1.2rem, 10vw, 11rem);
        font-weight: 400;
        margin-left: 1.5vw;
        position: relative;
        z-index: -1;
    }

    .hero-tags {
        display: flex;
        justify-content: space-between;
        width: 100%;
        margin-top: var(--size-4);
        font-size: clamp(0.9rem, 1.5vw, 1.5rem);
        letter-spacing: 0.05em;
        padding-left: 0vw;
    }

    .hero-tags .dash {
        opacity: 0.3;
    }
    .tag-container {
        position: relative;
        display: inline-block;
    }
    .tag-hidden {
        visibility: hidden;
    }

    .tag-scramble {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        text-align: center;
        white-space: nowrap;
    }

    .hero-image-wrapper {
        position: absolute;
        right: -2%;
        bottom: -35vh;
        height: 145vh;
        z-index: 1;
        pointer-events: none;
    }

    .hero-image {
        height: 100%;
        width: auto;
        object-fit: contain;
        object-position: bottom right;
    }

    .star {
        position: absolute;
        width: clamp(40px, 6vw, 90px);
        height: clamp(40px, 6vw, 90px);
        top: 29%;
        left: auto;
        right: 46vw;
        transform: rotate(-15deg);
        transform-origin: center center;
    }

    .marquee {
        padding-top: var(--size-6);
        padding-bottom: var(--size-2);
        overflow: hidden;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .marquee-content {
        display: flex;
        white-space: nowrap;
    }

    .marquee h2 {
        font-size: clamp(1.5rem, 3vw, 3rem);
        font-weight: 700;
        margin: 0;
        letter-spacing: 0.02em;
        color: var(--color-white, #ffffff);
    }

    .about-grid {
        display: grid;
        grid-template-columns: 1fr 1.2fr;
        gap: var(--size-4);
        align-items: center;
        overflow: visible;
        margin-top: -8vh;
    }

    .about-left {
        display: flex;
        justify-content: flex-start;
        align-items: center;
    }

    .star-text-wrapper {
        position: relative;
        width: 150%;
        max-width: 900px;
        aspect-ratio: 1;
        display: flex;
        justify-content: center;
        align-items: center;
        margin-left: -35%;
    }

    .bg-star {
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        z-index: 1;
        opacity: 0;
    }

    .about-title-inside {
        position: relative;
        z-index: 10;
        color: #ffffff; /* Testo bianco */
        display: flex;
        align-items: center;
        margin: 0;
        margin-top: 8%; /* Aumentato lo spazio sopra su desktop */
    }

    .script-a {
        font-family: var(--font-script);
        font-size: clamp(8rem, 14vw, 17rem);
        font-weight: 400;
        line-height: 0.8;
        padding-right: 0;
        margin-right: 0vw;
        padding-bottom: 2vh;
        z-index: 2;
    }

    .neulis-rest {
        font-family: var(--font-sans);
        font-size: clamp(3rem, 5vw, 6rem);
        text-transform: lowercase;
        font-weight: 400;
        line-height: 0.9;
        padding-left: 4vh;
        padding-top: 10vh;
    }

    .about-right {
        color: #000000;
        padding-right: var(--size-8);
        display: flex;
        flex-direction: column;
        gap: var(--size-8);
        position: relative;
        z-index: 10;
    }

    .description p {
        font-size: clamp(1.1rem, 1.8vw, 1.5rem);
        line-height: 1.5;
        margin: 0;
        font-weight: 400;
    }

    .description .right-align {
        text-align: right;
        padding-left: 10%;
    }

    /* --- NUOVI STILI COLLABORAZIONI SCROLL VERTICALE --- */
    .collaborations {
        padding-top: 4vh;
        padding-bottom: var(--size-12);
    }

    .collab-header {
        margin-bottom: var(--size-8);
    }

    .collab-title {
        display: flex;
        align-items: baseline;
        color: var(--color-brand);
        margin: 0;
        line-height: 0.8;
    }

    .letter-collab {
        font-family: var(--font-script);
        font-size: clamp(6rem, 12vw, 15rem);
        font-weight: 400;
        padding-right: 0;
        margin-right: -0.5vw;
    }

    .word-rest-collab {
        font-family: var(--font-sans);
        font-size: clamp(2.5rem, 5vw, 6rem);
        font-weight: 400;
        text-transform: lowercase;
    }

    .collab-vertical-list {
        display: flex;
        flex-direction: column;
        gap: var(--size-12);
    }

    .project-slide {
        display: flex;
        align-items: center;
        gap: 5vw;
        flex-wrap: wrap;
    }

    .phones-group {
        display: flex;
        gap: 2vw;
        position: relative;
        flex-wrap: wrap;
    }

    .collab-main-star {
        width: clamp(50px, 8vw, 90px);
        height: clamp(50px, 8vw, 90px);
        margin-left: 1.5vw;
        margin-top: 3.5vh;
        align-self: center;
    }

    /* CSS per replicare un telefono REALISTICO */
    .phone-mockup {
        width: clamp(200px, 22vw, 300px);
        aspect-ratio: 9 / 19.5;
        border-radius: 36px;
        position: relative;
        background: #000;
        padding: 6px;
        box-shadow:
            0 0 0 2px #d1d1d1,
            0 0 0 4px #a3a3a3,
            10px 20px 40px rgba(0, 0, 0, 0.15);
        flex-shrink: 0;
        cursor: pointer;
        transform-origin: center center;
    }

    /* Schermo interno */
    .phone-screen {
        width: 100%;
        height: 100%;
        background-color: #f4f4f4;
        border-radius: 30px;
        overflow: hidden;
        position: relative;
        transform: translateZ(
            0
        ); /* Fix per il clipping del video con bordi arrotondati */
    }

    .mockup-video {
        width: 100%;
        height: 100%;
        object-fit: cover;
        pointer-events: none; /* Impedisce al click di finire sul video stesso */
    }

    .play-overlay {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.4);
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 5;
        opacity: 0;
        transition: opacity 0.3s ease;
        pointer-events: none;
    }

    .phone-mockup:hover .play-overlay:not(.is-playing) {
        opacity: 1;
    }

    .play-overlay.is-playing {
        opacity: 0 !important;
    }

    /* La Notch stile Dynamic Island arrotondata */
    .phone-notch {
        position: absolute;
        top: 14px;
        left: 50%;
        transform: translateX(-50%);
        width: 30%;
        height: 22px;
        background: #000;
        border-radius: 20px;
        z-index: 10;
    }

    /* Pulsanti laterali del telefono usando CSS */
    .phone-mockup::before {
        content: "";
        position: absolute;
        top: 100px;
        left: -4px;
        width: 4px;
        height: 45px;
        background: #a3a3a3;
        border-radius: 4px 0 0 4px;
    }
    .phone-mockup::after {
        content: "";
        position: absolute;
        top: 130px;
        right: -4px;
        width: 4px;
        height: 60px;
        background: #a3a3a3;
        border-radius: 0 4px 4px 0;
    }

    .project-info {
        max-width: 400px;
        font-family: var(--font-sans);
        color: var(--color-text);
        flex: 1 1 300px;
    }

    .project-info h3 {
        font-size: clamp(1.2rem, 2vw, 1.8rem);
        margin-bottom: 1rem;
        font-weight: 700;
        display: inline-block;
        transition:
            transform 0.3s ease,
            color 0.3s ease;
    }

    .project-info h3:hover {
        transform: translateX(5px);
        color: var(--color-brand);
    }

    .project-info ul {
        list-style: none;
        padding: 0;
        display: flex;
        flex-direction: column;
        gap: 0.8rem;
    }

    .project-info li {
        position: relative;
        padding-left: 1.5rem;
        line-height: 1.4;
        font-size: clamp(1rem, 1.2vw, 1.2rem);
        transition:
            transform 0.3s ease,
            color 0.3s ease;
        cursor: default;
    }

    .project-info li:hover {
        transform: translateX(8px);
        color: var(--color-brand);
    }

    .project-info li::before {
        content: "•";
        position: absolute;
        left: 0;
        color: var(--color-brand);
        font-size: 1.5em;
        line-height: 0.8;
    }

    /* --- REASONS STYLES --- */
    .section-title-reasons {
        font-size: clamp(2rem, 4vw, 4.5rem);
        color: var(--color-white);
        margin-bottom: var(--size-6);
        display: flex;
        align-items: baseline;
        gap: 0.5rem;
        font-family: var(--font-sans);
        font-weight: 400;
    }

    .letter-script-reasons {
        font-size: clamp(4rem, 7vw, 7.5rem);
        line-height: 0.5;
        padding-right: 0.2rem;
    }

    .question-mark {
        font-size: clamp(3rem, 6vw, 6.5rem);
        line-height: 0.5;
        padding-left: 0.5rem;
    }

    .reasons-container {
        position: relative;
        margin-top: var(--size-6);
        border-top: 1px solid rgba(255, 255, 255, 0.5);
    }

    .reasons-bg-star {
        position: absolute;
        right: 5%;
        top: 45%;
        transform: translateY(-50%);
        width: clamp(100px, 25vw, 300px);
        height: clamp(100px, 25vw, 300px);
        opacity: 0.9;
        pointer-events: none;
        z-index: 0;
    }

    .reasons-list {
        display: flex;
        flex-direction: column;
        position: relative;
        z-index: 1;
    }

    .reason {
        border-bottom: 1px solid rgba(255, 255, 255, 0.5);
        padding: var(--size-6) var(--size-4);
        cursor: default;
        transition: background-color 0.4s ease;
    }

    .reason:hover {
        background-color: rgba(255, 255, 255, 0.05);
    }

    .reason h3 {
        font-size: clamp(1.5rem, 3vw, 2.5rem);
        margin: 0;
        font-weight: 800;
        color: var(--color-white);
        transition: transform 0.4s cubic-bezier(0.25, 1, 0.5, 1);
    }

    .reason:hover h3 {
        transform: translateX(15px);
    }

    .reason-content {
        display: grid;
        grid-template-rows: 0fr;
        transition: grid-template-rows 0.5s cubic-bezier(0.25, 1, 0.5, 1);
        overflow: hidden;
    }

    .reason:hover .reason-content,
    .reason.is-open .reason-content {
        grid-template-rows: 1fr;
    }

    .reason-content p {
        min-height: 0;
        margin: 0;
        padding-top: 0;
        opacity: 0;
        transition:
            opacity 0.4s ease,
            padding-top 0.5s cubic-bezier(0.25, 1, 0.5, 1),
            transform 0.4s ease;
        font-size: clamp(1rem, 1.2vw, 1.2rem);
        line-height: 1.5;
        max-width: 70%;
        color: rgba(255, 255, 255, 0.9);
        transform: translateY(-10px);
        font-family: var(--font-sans);
    }

    .reason:hover .reason-content p,
    .reason.is-open .reason-content p {
        padding-top: var(--size-4);
        opacity: 1;
        transform: translateY(0);
        transition-delay: 0.1s;
    }
    /* --- INSIGHTS STYLES --- */
    .insights {
        background: var(--color-white);
        color: var(--color-text);
        padding-top: var(--size-8); /* Ridotto leggermente */
        padding-bottom: var(--size-8); /* Ridotto */
    }

    .insights-title {
        color: var(--color-brand);
        display: flex;
        align-items: baseline;
        margin-bottom: var(--size-10);
        font-family: var(--font-sans);
        font-weight: 400;
    }

    .letter-script-insights {
        font-size: clamp(5rem, 8vw, 8.5rem);
        line-height: 0.5;
    }

    .word-rest-insights {
        font-size: clamp(2.5rem, 4vw, 4.5rem);
        line-height: 0.8;
    }

    .insights-grid {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: var(--size-10);
    }

    .insight-block h3 {
        font-size: clamp(1.2rem, 1.8vw, 2rem);
        font-weight: 800;
        margin-bottom: var(--size-6);
        color: var(--color-text);
        font-family: var(--font-sans);
    }

    .ig-data-grid {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: var(--size-6);
    }

    .ig-col {
        display: flex;
        flex-direction: column;
        gap: var(--size-5);
    }

    .mt-auto {
        margin-top: auto;
    }

    .tk-data-grid {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: var(--size-6); /* Usa lo stesso gap di ig-data-grid */
    }

    .data-item {
        display: flex;
        flex-direction: column;
        gap: 0.2rem;
    }

    .data-label {
        font-size: clamp(0.9rem, 1vw, 1.1rem);
        color: var(--color-text);
        font-weight: 500;
        font-family: var(--font-sans);
    }

    .data-value {
        font-size: clamp(3rem, 4.5vw, 4.5rem);
        font-weight: 800;
        color: var(--color-brand);
        line-height: 1;
        letter-spacing: -2px;
        font-family: var(--font-sans);
    }

    .label-city {
        font-size: clamp(1.8rem, 2.5vw, 2.8rem);
        letter-spacing: -1px;
    }

    .text-small {
        font-size: clamp(1.2rem, 1.8vw, 1.8rem);
        font-weight: 800;
    }

    .donut-chart-container {
        display: flex;
        align-items: center;
        gap: 1rem;
        margin-top: 1rem;
    }

    .donut-svg {
        width: 80px;
        height: 80px;
        transform: rotate(0deg);
    }

    .donut-label {
        font-size: clamp(0.8rem, 1vw, 1rem);
        font-weight: 800;
        color: var(--color-brand);
        max-width: 80px;
        line-height: 1.2;
    }

    /* === RESPONSIVE MOBILE CSS === */
    @media (max-width: 768px) {
        .intro-text {
            position: absolute;
            font-family: var(--font-sans);
            font-weight: 600;

            /* Aumentato: minimo 1.8rem (circa 28px) e massimo 4.5rem (circa 72px) */
            font-size: clamp(1.8rem, 5vw, 4.5rem);

            color: var(--color-white, #ffffff);
            text-align: center;
            opacity: 0;
            padding: var(--size-4);
            max-width: 95vw;
            width: 100%;
            letter-spacing: -0.02em; /* Un valore leggermente più negativo rende il testo grande più compatto ed elegante */
        }
        .hero {
            margin-bottom: -8vh;
        }
        .title-wrapper {
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .hero h1 {
            margin-left: 0;
            padding-top: 10vh;
            align-items: center;
        }
        .word-creator {
            margin-left: 0;
            margin-top: 0;
        }
        .letter-script {
            font-size: clamp(6rem, 30vw, 10rem);
        }
        .word-rest {
            font-size: clamp(2.5rem, 15vw, 5rem);
        }
        .hero-tags {
            flex-direction: column;
            align-items: center;
            gap: 1rem;
            margin-top: 8vh;
            font-size: 1.2rem;
        }
        .hero-tags .dash {
            display: none;
        }
        .star {
            position: relative;
            top: auto;
            left: auto;
            right: auto;
            margin-left: auto;
            margin-right: auto;
            margin-top: 4rem;
            z-index: 10;
            width: 120px;
            height: 120px;
        }
        .hero-image-wrapper {
            right: -5%;
            bottom: -5vh;
            height: 85vh;
            z-index: 0;
        }
        .hero-image {
            opacity: 0.4 !important;
        }
        .hero-content {
            z-index: 5;
            margin-top: -15vh;
        }

        .marquee {
            padding-top: 2vh;
        }
        .marquee-content {
            white-space: normal;
            text-align: center;
            padding: 0 1rem;
        }
        .marquee h2 {
            line-height: 1.4;
        }

        .about {
            margin-top: -2px;
            padding-bottom: 10vh;
        }
        .about-grid {
            grid-template-columns: 1fr;
            margin-top: -2vh;
            gap: 0;
        }
        .star-text-wrapper {
            width: 100%;
            margin-left: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            padding-block: 0;
            aspect-ratio: 1;
        }
        .bg-star {
            width: 140%;
            height: 140%;
            left: -20%;
            top: +50%;
        }
        .about-title-inside {
            margin-top: -13vh; /* Meno negativo = più spazio sopra su mobile */
            justify-content: center;
            align-items: center;
        }
        .script-a {
            font-size: clamp(6rem, 25vw, 10rem);
            padding-bottom: 0;
        }
        .neulis-rest {
            padding-top: 2vh;
            font-size: 1.6rem;
        }

        .about-right {
            margin-top: -13vh;
            padding-inline: 0rem;
            z-index: 10;
            gap: 1rem; /* Aumenta lo spazio tra i paragrafi */
        }
        .description p {
            font-size: 0.7rem;
            text-align: center;
            line-height: 1.6;
        }
        .description .right-align {
            text-align: center;
            padding-left: 0;
        }
        .neulis-rest br {
            display: inline;
            content: " ";
        }

        /* === Collaborazioni: Telefoni separati e proporzionati === */
        .letter-collab {
            font-size: clamp(
                4rem,
                15vw,
                6rem
            ); /* Rimpicciolisce la "C" corsiva */
        }

        .word-rest-collab {
            font-size: clamp(
                1.5rem,
                7vw,
                2.5rem
            ); /* Rimpicciolisce "ollaborazioni" per farlo stare nello schermo */
        }
        .project-slide {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 1.5rem;
            align-items: start;
        }

        .phones-group {
            display: contents; /* Make phones direct children of grid */
        }

        .phone-mockup {
            width: 100%;
            max-width: 160px; /* Un po' più grande per riempire la colonna */
            justify-self: center; /* Centra nella colonna */
            margin-left: 0;
            flex-shrink: 0;
            z-index: 1;
            /* Proporzioni corrette per sembrare veri iPhone */
            border-radius: 18px;
            padding: 4px;
            box-shadow:
                0 0 0 1px #d1d1d1,
                0 0 0 3px #a3a3a3,
                5px 10px 20px rgba(0, 0, 0, 0.15);
        }

        .phone-screen {
            border-radius: 14px; /* Adattato al nuovo bordo */
        }

        .phone-notch {
            top: 8px;
            height: 12px;
            border-radius: 10px;
        }

        .collaborations {
            padding-bottom: 2rem;
        }

        /* Pulsanti laterali del telefono usando CSS */
        .phone-mockup::before {
            content: "";
            position: absolute;
            top: 40px;
            left: -2px;
            width: 2px;
            height: 20px;
            background: #a3a3a3;
            border-radius: 4px 0 0 4px;
        }
        .phone-mockup::after {
            content: "";
            position: absolute;
            top: 55px;
            right: -2px;
            width: 2px;
            height: 35px;
            background: #a3a3a3;
            border-radius: 0 4px 4px 0;
        }

        .collab-main-star {
            display: none;
        }

        .project-info h3 {
            padding-top: 2rem;
            font-size: 0.95rem; /* Rimpicciolito il titolo LENOVO */
            margin-bottom: 0.3rem; /* Avvicinato alla lista */
        }

        .project-info ul {
            align-items: flex-start;
            text-align: left;
            /* AGGIUNGI QUESTA RIGA: annulla lo spazio gigante tra un punto e l'altro */
            gap: 0.4rem;
        }

        .project-info li {
            font-size: 0.6rem; /* Rimpicciolito il testo dei punti elenco */
            padding-left: 0.8rem; /* Avvicinato il pallino al testo */
            margin-bottom: 0;
            line-height: 1.2; /* Compatta le righe quando un punto va a capo */
        }

        .collab-vertical-list {
            gap: 4rem;
        }

        /* Insights Tweak */
        .insights-grid {
            grid-template-columns: 1fr;
            margin-left: -1.5rem;
        }
        .ig-data-grid {
            grid-template-columns: 1fr 1fr;
            gap: 3.3rem;
        }
        .ig-col-right {
            margin-top: 0;
        }

        /* === Rimpicciolisce il titolo "Social Insights" su Mobile === */
        .letter-script-insights {
            font-size: clamp(
                5rem,
                12vw,
                8rem
            ); /* Rimpicciolisce la S e la I maiuscole */
        }

        .word-rest-insights {
            font-size: clamp(
                1.5rem,
                6vw,
                2rem
            ); /* Rimpicciolisce "ocial" e "nsights" */
        }

        /* === Rimpicciolisce i numeri blu degli Insights su Mobile === */
        .data-value {
            font-size: clamp(
                1.8rem,
                8vw,
                2.5rem
            ); /* Rimpicciolisce i numeri grandi (es. +976%, 113.7K) */
            letter-spacing: -1px; /* Adegua lo spazio tra i numeri per la nuova dimensione */
        }

        /* === Rimpicciolisce le scritte in bold (etichette dei dati) === */
        .data-label {
            font-size: 0.74rem; /* Riduce le scritte sopra i numeri blu (es. Account Raggiunti) */
            font-weight: 700; /* Mantiene l'aspetto bold ma più compatto */
        }

        /* === Rimpicciolisce i titoli dei blocchi (Instagram... / TikTok...) === */
        .insight-block h3 {
            font-size: 1.1rem; /* Riduce i titoli delle due sezioni */
            margin-bottom: 1rem; /* Regola di conseguenza lo spazio sotto il titolo */
        }

        /* Sistema anche la label interna alla ciambella se serve */
        .donut-label {
            font-size: 0.75rem; /* Rimpicciolisce la scritta "Non-Follower" */
        }

        .label-city {
            font-size: clamp(
                1.4rem,
                6vw,
                1.8rem
            ); /* Rimpicciolisce la parola "Milano" */
        }

        .text-small {
            font-size: clamp(
                1rem,
                4vw,
                1.2rem
            ); /* Rimpicciolisce le percentuali tra parentesi (es. 7.5%) */
        }

        .donut-label .num-animate {
            font-size: clamp(
                1.2rem,
                5vw,
                1.5rem
            ); /* Rimpicciolisce il numero nel grafico a ciambella */
        }

        /* === Riduce lo spazio sotto il titolo "Social Insights" === */
        .insights-title {
            margin-bottom: 2rem; /* Abbassa questo valore (es. 1.5rem) se lo vuoi ancora più vicino */
        }

        /* Opzionale: se vuoi ridurre anche lo spazio sotto "Instagram Analytics" prima dei dati */
        .insight-block h3 {
            margin-bottom: 1.5rem;
        }

        /* === Riduce lo spazio tra il blocco Instagram e TikTok === */
        .insights-grid {
            gap: 3rem; /* Cambia a 1.5rem se lo vuoi ancora più vicino */
        }
    }

    /* === STILI FINESTRA VIDEO TUTTO SCHERMO === */
    .video-modal {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background: #000000; /* Nero assoluto per simulare app native */
        z-index: 10000;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .close-btn {
        position: absolute;
        top: 40px;
        right: 20px;
        background: rgba(255, 255, 255, 0.2);
        border: none;
        color: white;
        font-size: 1.5rem;
        width: 45px;
        height: 45px;
        border-radius: 50%;
        cursor: pointer;
        z-index: 10001;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .fullscreen-video {
        width: 100vw;
        height: 100vh;
        max-width: none;
        aspect-ratio: auto;
        border-radius: 0;
        object-fit: contain; /* Video a tutto schermo senza tagliare l'immagine */
        box-shadow: none;
    }
</style>
