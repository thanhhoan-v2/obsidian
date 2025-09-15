```js
import { useGSAP } from "@gsap/react";
import gsap from "gsap";
```

```html
<div>
  <div id="blue-box" className="w-20 h-20 bg-blue-500 rounded-lg" />
</div>
```

## gsap.to()

> The `gsap.to()` method is used to animate elements from their current state to a new state.

The `gsap.to()` method is similar to the `gsap.from()` method, but the difference is that the `gsap.to()` method animates elements from their current state to a new state, while the `gsap.from()` method animates elements from a new state to their current state.

Read more about the [gsap.to()](<https://greensock.com/docs/v3/GSAP/gsap.to()>) method.

```js
useGSAP(() => {
  gsap.to("#blue-box", {
    x: 200,
    duration: 3,
    repeat: -1,
    yoyo: true,
    rotation: 360,
    ease: "elastic",
  });
}, []);
```

## gsap.from()

> The `gsap.from()` method is used to animate elements from a new state to their current state.

The `gsap.from()` method is similar to the `gsap.to()` method, but the difference is that the `gsap.from()` method animates elements from a new state to their current state, while the `gsap.to()` method animates elements from their current state to a new state.

Read more about the [gsap.from()](<https://greensock.com/docs/v3/GSAP/gsap.from()>) method.

```js
useGSAP(() => {
  gsap.from("#blue-box", {
    x: 200,
    duration: 3,
    repeat: -1,
    yoyo: true,
    rotation: 360,
    ease: "power1.inOut",
  });
}, []);
```

## gsap.fromTo()

> The `gsap.fromTo()` method is used to animate elements from a new state to a new state.

The `gsap.fromTo()` method is similar to the `gsap.from()` and `gsap.to()` methods, but the difference is that the `gsap.fromTo()` method animates elements from a new state to a new state, while the `gsap.from()` method animates elements from a new state to their current state, and the `gsap.to()` method animates elements from their current state to a new state.

Read more about the [gsap.fromTo()](<https://greensock.com/docs/v3/GSAP/gsap.fromTo()>) method.

```js
useGSAP(() => {
  gsap.fromTo(
    "#blue-box",
    {
      x: 0,
      rotation: 0,
      borderRadius: "0%",
    },
    {
      x: 200,
      rotation: 360,
      borderRadius: "360%",
      duration: 3,
      repeat: -1,
      yoyo: true,
      ease: "power1.inOut",
    },
  );
}, []);
```

## gsap.timeline()

> The `gsap.timeline()` method is used to create a timeline instance that can be used to manage multiple animations.

The `gsap.timeline()` method is similar to the `gsap.to()`, `gsap.from()`, and `gsap.fromTo()` methods, but the difference is that the `gsap.timeline()` method is used to create a timeline instance that can be used to manage multiple animations, while the `gsap.to()`, `gsap.from()`, and `gsap.fromTo()` methods are used to animate elements from their current state to a new state, from a new state to their current state, and from a new state to a new state, respectively.

Read more about the [gsap.timeline()](<https://greensock.com/docs/v3/GSAP/gsap.timeline()>) method.

```js
const timeline = gsap.timeline({ repeat: -1, repeatDelay: 1, yoyo: true });

useGSAP(() => {
  timeline.from("#blue-box", {
    x: 200,
    duration: 1,
    ease: "elastic.out(1, 0.3)",
  });

  timeline.to("#blue-box", {
    x: 0,
    duration: 1,
    ease: "elastic.out(1, 0.3)",
  });

  timeline.to("#blue-box", {
    x: 200,
    duration: 1,
    ease: "elastic.out(1, 0.3)",
  });
}, []);
```

## stagger

> GSAP stagger is a feature that allows you to apply animations with a staggered delay to a group of elements.

By using the stagger feature in GSAP, you can specify the amount of time to stagger the animations between each element, as well as customize the easing and duration of each individual animation. This enables you to create dynamic and visually appealing effects, such as staggered fades, rotations, movements, and more.

Read more about the [Gsap Stagger](https://gsap.com/resources/getting-started/Staggers) feature.

```js
useGSAP(() => {
  gsap.to(".stagger-box", {
    y: 200,
    ease: "elastic.out(1, 0.3)",
    repeat: -1,
    yoyo: true,
    stagger: {
      amount: 2,
      grid: [3, 1],
      axis: "y",
      from: "center",
      ease: "circ.inOut",
    },
  });
}, []);
```

```html
<div className="mt-20">
  <div className="flex gap-5">
    <div className="w-20 h-20 bg-indigo-200 rounded-lg stagger-box" />
    <div className="w-20 h-20 bg-indigo-300 rounded-lg stagger-box" />
    <div className="w-20 h-20 bg-indigo-400 rounded-lg stagger-box" />
    <div className="w-20 h-20 bg-indigo-500 rounded-lg stagger-box" />
    <div className="w-20 h-20 bg-indigo-600 rounded-lg stagger-box" />
    <div className="w-20 h-20 bg-indigo-700 rounded-lg stagger-box" />
    <div className="w-20 h-20 bg-indigo-800 rounded-lg stagger-box" />
  </div>
</div>
```

## scrollTrigger

> Gsap Scroll Trigger is a plugin that allows you to create animations that are triggered by the scroll position of the page.

With ScrollTrigger, you can define various actions to be triggered at specific scroll points, such as starting or ending an animation, scrubbing through animations as the user scrolls, pinning elements to the screen, and more.

Read more about the [gsap scroll trigger](https://gsap.com/docs/v3/Plugins/ScrollTrigger/) method.

```js
import { ScrollTrigger } from "gsap/all";
import gsap from "gsap";

gsap.registerPlugin(ScrollTrigger);
```

```js
const scrollRef = useRef();

useGSAP(
  () => {
    const boxes = gsap.utils.toArray(scrollRef.current.children);
    boxes.forEach((box, idx) => {
      gsap.to(box, {
        x: 100 * (idx * 5),
        duration: 1,
        rotate: 360,
        borderRadius: "50%",
        scale: 1.5,
        scrollTrigger: {
          trigger: box,
          start: "bottom bottom",
          end: "top 20%",
          scrub: true,
        },
      });
    });
  },
  { scope: scrollRef },
);
```

```html
<div ref="{scrollRef}" className="min-h-screen">
  <div className="w-20 h-20 bg-red-500 rounded-lg mb-10" />
  <div className="w-20 h-20 bg-green-500 rounded-lg mb-10" />
  <div className="w-20 h-20 bg-blue-500 rounded-lg mb-10" />
  <div className="w-20 h-20 bg-yellow-500 rounded-lg mb-10" />
</div>
```

## text

> We can use same method like `gsap.to()`, `gsap.from()`, `gsap.fromTo()` and `gsap.timeline()` to animate text.

Using these methods we can achieve various text animations and effects like fade in, fade out, slide in, slide out, and many more.

For more advanced text animations and effects, you can explore the GSAP TextPlugin or other third-party libraries that specialize in text animations.

Read more about the [TextPlugin](https://greensock.com/docs/v3/Plugins/TextPlugin) plugin.

```html
<h1 id="text" className="opacity-0 translate-y-10">GsapText</h1>
<p className="mt-5 text-gray-500 para">
  We can use same method like <code>gsap.to()</code>,{" "}
  <code>gsap.from()</code>, <code>gsap.fromTo()</code> and{" "}
  <code>gsap.timeline()</code> to animate text.
</p>
<p className="mt-5 text-gray-500 para">
  Using these methods we can achieve various text animations and effects like
  fade in, fade out, slide in, slide out, and many more.
</p>
<p className="mt-5 text-gray-500 para">
  For more advanced text animations and effects, you can explore the GSAP
  TextPlugin or other third-party libraries that specialize in text animations.
</p>
```

```js
useGSAP(() => {
  gsap.to("#text", {
    ease: "power2.inOut",
    duration: 1,
    opacity: 1,
    y: 0,
  });

  gsap.fromTo(
    ".para",
    {
      opacity: 0,
      y: 20,
    },
    {
      opacity: 1,
      y: 0,
      delay: 1,
      stagger: 0.1,
    },
  );
});
```

## SplitText Plugin

> The SplitText plugin allows you to split text into individual characters, words, or lines for advanced text animations.

SplitText is a powerful GSAP plugin that breaks text into smaller parts, allowing you to animate each piece independently. This creates stunning text reveal effects and complex typography animations.

Read more about the [SplitText Plugin](https://greensock.com/docs/v3/Plugins/SplitText).

```js
import { SplitText } from "gsap/all";
import gsap from "gsap";

// Register the plugin
gsap.registerPlugin(SplitText);

useGSAP(() => {
  // Split text into characters and words
  const heroSplit = new SplitText(".title", {
    type: "chars, words",
  });

  // Split paragraph into lines
  const paragraphSplit = new SplitText(".subtitle", {
    type: "lines",
  });

  // Apply CSS classes to split elements
  heroSplit.chars.forEach((char) => char.classList.add("text-gradient"));

  // Animate characters with stagger
  gsap.from(heroSplit.chars, {
    yPercent: 100,
    duration: 1.8,
    ease: "expo.out",
    stagger: 0.06,
  });

  // Animate lines with delay
  gsap.from(paragraphSplit.lines, {
    opacity: 0,
    yPercent: 100,
    duration: 1.8,
    ease: "expo.out",
    stagger: 0.06,
    delay: 1,
  });
}, []);
```

```html
<div>
  <h1 className="title">MOJITO</h1>
  <p className="subtitle">
    Sip the Spirit <br />
    of Summer
  </p>
</div>
```

## ScrollTrigger with Pinning

> ScrollTrigger with pinning allows you to "stick" elements in place while other animations play out during scroll.

Pinning is useful for creating immersive scroll experiences where sections stay in view while content animates or transforms. This technique is perfect for storytelling and focus effects.

Read more about [ScrollTrigger Pinning](https://greensock.com/docs/v3/Plugins/ScrollTrigger).

```js
import { ScrollTrigger } from "gsap/all";
import { useMediaQuery } from "react-responsive";

gsap.registerPlugin(ScrollTrigger);

useGSAP(() => {
  const isMobile = useMediaQuery({ maxWidth: 767 });

  const startValue = isMobile ? "top 50%" : "center 60%";
  const endValue = isMobile ? "120% top" : "bottom top";

  const maskTimeline = gsap.timeline({
    scrollTrigger: {
      trigger: "#art",
      start: startValue,
      end: "bottom center",
      scrub: 1.5,
      pin: true, // Pin the section during animation
    },
  });

  maskTimeline
    .to(".will-fade", {
      opacity: 0,
      stagger: 0.2,
      ease: "power1.inOut",
    })
    .to(".masked-img", {
      scale: 1.3,
      maskPosition: "center",
      maskSize: "400%",
      duration: 1,
      ease: "power1.inOut",
    })
    .to("#masked-content", {
      opacity: 1,
      duration: 1,
      ease: "power1.inOut",
    });
}, []);
```

```html
<section id="art" className="min-h-screen relative">
  <h2 className="will-fade">The ART</h2>

  <ul className="will-fade">
    <li>Handpicked ingredients</li>
    <li>Signature techniques</li>
    <li>Bartending artistry</li>
  </ul>

  <div className="cocktail-img">
    <img src="/images/cocktail.jpg" alt="cocktail" className="masked-img" />
  </div>

  <div id="masked-content" className="opacity-0">
    <h3>Made with Craft, Poured with Passion</h3>
    <p>This isn't just a drink. It's a moment.</p>
  </div>
</section>
```

## Video Scroll Synchronization

> Sync video playback with scroll position to create cinematic storytelling effects.

This advanced technique allows you to control video playback based on user scroll, creating immersive experiences where the video plays forward or backward as the user scrolls.

```js
const videoRef = useRef();

useGSAP(() => {
  const isMobile = useMediaQuery({ maxWidth: 767 });
  const startValue = isMobile ? "top 50%" : "center 60%";
  const endValue = isMobile ? "120% top" : "bottom top";

  let tl = gsap.timeline({
    scrollTrigger: {
      trigger: "video",
      start: startValue,
      end: endValue,
      scrub: true,
      pin: true,
    },
  });

  // Sync video currentTime with scroll progress
  videoRef.current.onloadedmetadata = () => {
    tl.to(videoRef.current, {
      currentTime: videoRef.current.duration,
    });
  };
}, []);
```

```html
<div className="video absolute inset-0">
  <video
    ref="{videoRef}"
    muted
    playsinline
    preload="auto"
    src="/videos/output.mp4"
  />
</div>
```

feat(ui): implement full-screen landing video with transparent navbar and branding

- Make landing video fill the entire viewport (100vh), including behind the navbar
- Update navbar to be fully transparent on the home page and float above the video
- Enhance navbar branding with Star Wars and TalentGetGo logos and a separator icon
- Improve text and button contrast for readability over video
- Refactor hero section for a more cinematic, immersive experience
- Fix video loading by serving from public/ and referencing via direct path
- Add images following character's id

## Parallax Scrolling Effects

> Create depth and immersion by moving elements at different speeds during scroll.

Parallax effects add visual depth to your website by moving background and foreground elements at different rates, creating a sense of 3D space.

```js
useGSAP(() => {
  // Parallax timeline with ScrollTrigger
  gsap
    .timeline({
      scrollTrigger: {
        trigger: "#hero",
        start: "top top",
        end: "bottom top",
        scrub: true,
      },
    })
    .to(".right-leaf", { y: 200 }, 0)
    .to(".left-leaf", { y: -200 }, 0)
    .to(".arrow", { y: 100 }, 0);

  // Simple parallax for decorative elements
  const parallaxTimeline = gsap.timeline({
    scrollTrigger: {
      trigger: "#cocktails",
      start: "top 30%",
      end: "bottom 80%",
      scrub: true,
    },
  });

  parallaxTimeline
    .from("#c-left-leaf", {
      x: -100,
      y: 100,
    })
    .from("#c-right-leaf", {
      x: 100,
      y: 100,
    });
}, []);
```

```html
<section id="hero" className="min-h-screen relative">
  <img src="/images/hero-left-leaf.png" className="left-leaf" />
  <img src="/images/hero-right-leaf.png" className="right-leaf" />
  <img src="/images/arrow.png" className="arrow" />
</section>

<section id="cocktails" className="min-h-screen relative">
  <img src="/images/cocktail-left-leaf.png" id="c-left-leaf" />
  <img src="/images/cocktail-right-leaf.png" id="c-right-leaf" />
</section>
```

## Image Masking with Scroll

> Use CSS masks combined with GSAP to create stunning reveal effects.

Image masking allows you to create sophisticated reveal effects where images appear through custom shapes or patterns, controlled by scroll position.

```css
.masked-img {
  mask-image: url("/images/mask-img.png");
  mask-repeat: no-repeat;
  mask-position: center;
  mask-size: 50%;
}
```

```js
useGSAP(() => {
  const maskTimeline = gsap.timeline({
    scrollTrigger: {
      trigger: "#art",
      start: "top top",
      end: "bottom center",
      scrub: 1.5,
      pin: true,
    },
  });

  maskTimeline.to(".masked-img", {
    scale: 1.3,
    maskPosition: "center",
    maskSize: "400%", // Expand mask size
    duration: 1,
    ease: "power1.inOut",
  });
}, []);
```

```html
<div className="cocktail-img relative">
  <img
    src="/images/under-img.jpg"
    alt="cocktail"
    className="masked-img w-full h-full object-cover"
  />
</div>
```

## Background Property Animations

> Animate CSS background properties like backgroundColor for dynamic UI changes.

You can animate various CSS properties including background colors, which is useful for creating dynamic navigation bars or section transitions.

```js
useGSAP(() => {
  const navTween = gsap.timeline({
    scrollTrigger: {
      trigger: "nav",
      start: "bottom top",
    },
  });

  navTween.fromTo(
    "nav",
    { backgroundColor: "transparent" },
    {
      backgroundColor: "#00000050",
      backgroundFilter: "blur(10px)",
      duration: 1,
      ease: "power1.inOut",
    },
  );
});
```

```html
<nav className="fixed w-full z-50">
  <div className="container mx-auto flex justify-between items-center py-5">
    <a href="#home" className="flex items-center gap-2">
      <img src="/images/logo.png" alt="logo" />
      <p>Velvet Pour</p>
    </a>

    <ul className="flex gap-8">
      <li><a href="#cocktails">Cocktails</a></li>
      <li><a href="#about">About Us</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>
  </div>
</nav>
```

## Responsive GSAP Animations

> Create adaptive animations that respond to different screen sizes using media queries.

Combining GSAP with responsive design ensures your animations work perfectly across all devices by adjusting values based on screen size.

```js
import { useMediaQuery } from "react-responsive";

useGSAP(() => {
  const isMobile = useMediaQuery({ maxWidth: 767 });

  // Adjust animation values based on screen size
  const startValue = isMobile ? "top 50%" : "center 60%";
  const endValue = isMobile ? "120% top" : "bottom top";

  gsap.timeline({
    scrollTrigger: {
      trigger: "video",
      start: startValue,
      end: endValue,
      scrub: true,
      pin: true,
    },
  });
}, []);
```

```html
<div className="container">
  {/* Desktop layout */}
  <div className="hidden md:block">
    <video ref="{videoRef}" src="/videos/cocktail.mp4" />
  </div>

  {/* Mobile layout */}
  <div className="md:hidden">
    <video ref="{videoRef}" src="/videos/cocktail-mobile.mp4" />
  </div>
</div>
```

## Complex State-Driven Animations

> Combine GSAP with React state to create interactive animations that respond to user actions.

This technique is perfect for carousels, sliders, and interactive components where animations need to respond to state changes.

```js
const [currentIndex, setCurrentIndex] = useState(0);

useGSAP(() => {
  // Re-run animations when state changes
  gsap.fromTo("#title", { opacity: 0 }, { opacity: 1, duration: 1 });

  gsap.fromTo(
    ".cocktail img",
    { opacity: 0, xPercent: -100 },
    { xPercent: 0, opacity: 1, duration: 1, ease: "power1.inOut" },
  );

  gsap.fromTo(
    ".details h2",
    { yPercent: 100, opacity: 0 },
    { yPercent: 0, opacity: 100, ease: "power1.inOut" },
  );
}, [currentIndex]); // Dependency array triggers re-run

const goToSlide = (index) => {
  const newIndex = (index + totalCocktails) % totalCocktails;
  setCurrentIndex(newIndex);
};
```

```html
<section id="menu" className="relative">
  <nav className="cocktail-tabs grid grid-cols-4 gap-10">
    <button className="text-white border-b">Classic Mojito</button>
    <button className="text-white/50 border-white/50">Raspberry Mojito</button>
    <button className="text-white/50 border-white/50">Violet Breeze</button>
    <button className="text-white/50 border-white/50">Curacao Mojito</button>
  </nav>

  <div className="content">
    <div className="cocktail">
      <img src="/images/drink1.png" className="h-96 object-contain" />
    </div>

    <div className="recipe">
      <div className="info">
        <p>Recipe for:</p>
        <p id="title">Classic Mojito</p>
      </div>

      <div className="details">
        <h2>Simple Ingredients, Bold Flavor</h2>
        <p>Made with fresh mint, lime, and premium rum...</p>
      </div>
    </div>
  </div>
</section>
```

## Multiple Element Selection

> Use gsap.utils.toArray to animate multiple elements efficiently.

When you need to animate multiple similar elements, `gsap.utils.toArray` provides a convenient way to select and animate them with different properties.

```js
const scrollRef = useRef();

useGSAP(
  () => {
    const boxes = gsap.utils.toArray(scrollRef.current.children);

    boxes.forEach((box, idx) => {
      gsap.to(box, {
        x: 100 * (idx * 5),
        duration: 1,
        rotate: 360,
        borderRadius: "50%",
        scale: 1.5,
        scrollTrigger: {
          trigger: box,
          start: "bottom bottom",
          end: "top 20%",
          scrub: true,
        },
      });
    });
  },
  { scope: scrollRef },
);
```

```html
<div ref="{scrollRef}" className="grid grid-cols-3 gap-5">
  <div className="w-20 h-20 bg-purple-500 rounded-lg" />
  <div className="w-20 h-20 bg-pink-500 rounded-lg" />
  <div className="w-20 h-20 bg-indigo-500 rounded-lg" />
  <div className="w-20 h-20 bg-cyan-500 rounded-lg" />
  <div className="w-20 h-20 bg-teal-500 rounded-lg" />
  <div className="w-20 h-20 bg-orange-500 rounded-lg" />
</div>
```

## Advanced Timeline Sequencing

> Create complex animation sequences with precise timing control using timelines.

Timelines allow you to create sophisticated animation sequences with overlapping animations, delays, and precise timing control.

```js
useGSAP(() => {
  const timeline = gsap.timeline({
    scrollTrigger: {
      trigger: "#contact",
      start: "top center",
    },
    ease: "power1.inOut",
  });

  timeline
    .from(titleSplit.words, {
      opacity: 0,
      yPercent: 100,
      stagger: 0.02,
    })
    .from("#contact h3, #contact p", {
      opacity: 0,
      yPercent: 100,
      stagger: 0.02,
    })
    .to("#f-right-leaf", {
      y: "-50",
      duration: 1,
      ease: "power1.inOut",
    })
    .to(
      "#f-left-leaf",
      {
        y: "-50",
        duration: 1,
        ease: "power1.inOut",
      },
      "<",
    ); // '<' starts at the same time as previous animation
});
```

```html
<footer id="contact" className="relative min-h-screen">
  <img src="/images/footer-right-leaf.png" id="f-right-leaf" />
  <img src="/images/footer-left-leaf.png" id="f-left-leaf" />

  <div className="content">
    <h2>Where to Find Us</h2>

    <div>
      <h3>Visit Our Bar</h3>
      <p>456, Raq Blvd. #404, Los Angeles, CA 90210</p>
    </div>

    <div>
      <h3>Contact Us</h3>
      <p>(555) 987-6543</p>
      <p>hello@jsmcocktail.com</p>
    </div>
  </div>
</footer>
```

## Plugin Registration

> Always register GSAP plugins before using them in your application.

Plugin registration should be done once in your main App component to ensure all GSAP features are available throughout your application.

```js
import gsap from "gsap";
import { ScrollTrigger, SplitText } from "gsap/all";

// Register plugins once in your main App component
gsap.registerPlugin(ScrollTrigger, SplitText);

const App = () => {
  return <main>{/* Your components */}</main>;
};
```

```html
<div id="app">
  <nav><!-- Navigation --></nav>
  <main>
    <section id="hero"><!-- Hero Section --></section>
    <section id="cocktails"><!-- Cocktails Section --></section>
    <section id="about"><!-- About Section --></section>
    <section id="contact"><!-- Contact Section --></section>
  </main>
</div>
```
