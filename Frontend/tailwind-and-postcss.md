postcss och tailwindcss

Postcss är ett javascriptlib som skriver om din css.

Detta är den nyare tekniken, och den gör sass och less onödigt har jag läst.
Postcss kan användas tillsammans med sass/less men verkar som att det skulle kunna krävas lite konfigurerande. Postcss är iallafall framtiden och sass/less blir obsolet. Du kan göra egna plugins till Postcss precis som Tailwindcss, och du kan göra plugins till Tailwindcss också.

Vi har referens till postcss samt ett par plugins i package.json i aftenweb i dag - dock verkar inget av det användas någonstans i projektet än.

Tailwind är ett plugin till postcss som skänker utvecklaren två funktioner och de är;
1. En gedigen lista av fördefinerade css-klasser inkl kombinationer med olika utilities och pseudo-classes.
2. AtRule-direktiv (@apply myclass-1 myclass-2)

--- 1. Fördefinerade klasser ---

But using utility classes has a few important advantages over inline styles:

    Designing with constraints. Using inline styles, every value is a magic number. With utilities, you're choosing styles from a predefined design system, which makes it much easier to build visually consistent UIs.
		
    Responsive design. You can't use media queries in inline styles, but you can use Tailwind's responsive utilities to build fully responsive interfaces easily.
		t.ex: sm:text-white, md:text-gray
		
    Pseudo-classes. Inline styles can't target states like hover or focus, but Tailwind's pseudo-class variants make it easy to style those states with utility classes.
		t.ex: hover:text-white, focus:outline-none, visited:bg-blue-500

--- 2. AtRule-direktiv ---

Syftet är att skriva läsbar kod. Semantisk kod.

Här ser vi att klasserna för knappen beskriver layouten, utseendet, mer än förklarar syftet för knappen.
<!-- Using utilities -->
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
  Button
</button>

Medan här har man kortat ner knappens beskrivande klasser till 'btn-blue', jag hade nog beskrivit knappen med klass 'btn-submit' eller 'btn-book-service' för då vet man vad den gör.
<!-- Extracting classes using @apply -->
<button class="btn-blue">
  Button
</button>

Och då slänger man på alla klasser (från tailwind eller egna) med ett custom atrule directive:
<style>
  .btn {
    @apply font-bold py-2 px-4 rounded;
  }
  .btn-blue {
    @apply bg-blue-500 text-white;
  }
  .btn-blue:hover {
    @apply bg-blue-700;
  }
</style>


Jobbig kod:
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
  Button
</button>

Skön kod:
  <button class="btn btn-blue">
    Button
  </button>

  <style>
		.btn-blue {
			@apply bg-blue-500 text-white font-bold py-2 px-4 rounded;
		}
		.btn-blue:hover {
			@apply bg-blue-700;
		}
  </style>
	
	-----------------------------------------------------------
	
	<!-- importera de du behöver -->
	
	/* Using postcss-import */
	@import "tailwindcss/base";
	@import "tailwindcss/components";
	@import "tailwindcss/utilities";
	@import "./custom-utilities.css";

	/* Using Sass or Less */
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
	@import "./custom-utilities";
	
	---   ---   ---   ---
	
	Breakpoints
	
	@screen sm {
		/* ... */
	}
	@screen tablet {
		/* ... */
	}
	
	---   ---   ---   ---
	
	// tailwind.config.js
	module.exports = {
		theme: {
			screens: {
				'tablet': '640px',
				// => @media (min-width: 640px) { ... }

				'laptop': '1024px',
				// => @media (min-width: 1024px) { ... }

				'desktop': '1280px',
				// => @media (min-width: 1280px) { ... }
			},
		}
	}
 
	---   ---   ---   ---
	
	// tailwind.config.js
	module.exports = {
		theme: {
			screens: {
				'sm': {'max': '639px'},
				// => @media (max-width: 639px) { ... }

				'md': {'max': '767px'},
				// => @media (max-width: 767px) { ... }

				'lg': {'max': '1023px'},
				// => @media (max-width: 1023px) { ... }

				'xl': {'max': '1279px'},
				// => @media (max-width: 1279px) { ... }
			}
		}
	}
