## Lecture : Creating Cool CSS Animations:

### This can be done in 2 ways,

- Animate using the transition property and then change the properties that we want to animate on an event like hover
- Animate using keyframes

> NOTE - for browser performance it is best to only ever animate two different properties, one is opacity and other is transform property, that's what most browsers are optimized for - With transform we can do a lot

## Lecture : Building a complex animated button

### inline vs inline-block

- While both inline and inline-block elements can flow alongside text and other inline elements, inline-block elements offer more control over layout properties such as width, height, padding, and margins.
- Whereas inline elements do not start on a new line and only occupy as much width as necessary. You cannot set width and height properties on inline elements.
- We should change inline elements to inline-block if we want to give those elements width, height, padding or margin properties

## Lecture : Three pillars of writing good HTML and CSS

- Responsive design - One website that look beautifully on all screen sizes on all devices fluid layouts, media queries, responsive images, correct units, desktop first vs mobile first
- Maintainable and scalable code - clean, easy to understand, supports future growth, reusable, following class naming convention, how to structure HTML, how to organize files
- Caring about Web performance - Making an app more performant is to make the app more faster and smaller in size - make less HTTP requests as possible, less code, compress code, use a css preprocessor, less images, compress images (use only images that are necessary for the web app - compressed images use less bandwidth from the user end)

## How CSS works behind the scenes : An overview

### What happens to CSS when we load up a webpage

- When browser starts to load the initial html file - it then parses the HTML by decoding it line by line and build's browser DOM - Document Object Model (DOM) describes the entire web document in a tree structure - Here is where the entire decoded HTML is stored
- As the browser parses the HTML it also finds the style sheets in the HTML head and starts loading them as well - Just like HTML, CSS is also parsed but parsing of CSS is bit more complex - it involves 2 main steps

  - Resolving conflicting CSS declarations (cascade)
  - Process final CSS values - like converting margin defined from percentages to pixels (they can only be calculated on the user's device in parsing phase)

- The final CSS is also stored in a tree like structure called CSS Object Model(CSSOM)
- The HTML and CSS Object Model together form the so called render tree - the browser applies visual formatting model(algorithm) to render a page - after the algorithm is executed the website is finally rendered on to the screen

## Lecture : How CSS is parsed, Part 1: The cascade and specificity

- CSS Rule consists of a selector (to select one or more html elements that we want to style) and a declaration block (where we write actual styles to style elements on the page) - to do so we can write one or more CSS declarations in each block - each declaration consists of a CSS property and its corresponding value (declared value)
- The Cascade (resolve conflicting CSS declarations) - is a process of combining different stylesheets and resolving conflicts between different CSS rules and declarations, when more than one rule applies to a certain element (like declaration for font size property can appear in several stylesheets and also multiple times inside one single stylesheet)
- CSS can also come from different sources
  - the most common one is the one written by the developers - these declarations that we write in our own stylesheets are called the 'author' declarations
  - another source can be the 'user' declarations which is the CSS coming from the user - like when user changes the default font size on the browser
  - there are also default browser declarations (user agent stylesheets) - like an anchor tag in HTML - usually be rendered in blue text with underlined
  - So cascade combine CSS coming from all these different sources - It resolves the conflict by looking at the importance(weight), selector specificity and source order of conflicting declarations to determine which one takes precedence over another

### Process of cascading -

- First cascade gives conflicting declarations different importances based on where they are declared (source)
  - most important declarations are user declarations marked with !important keyword
  - author declarations marked with !important
  - normal author declarations
  - normal user declarations
  - default browser declarations
- What happens when bunch of conflicting declarations have the same importance, what cascade does is to calculate and compare the specificity of the declaration selectors
  - inline styles have highest specificity
  - IDs
  - Classess, pseudo-classes, attribute
  - Elements, pesudo-elements selector
- The value of winning declaration is called cascaded value - which resulted from cascade
- If both have exact same specificity and same importance then the last CSS declaration that is written in the code will override all other declarations and will be applied

### Cascade and specificity: What you need to know

- CSS declarations marked with !important have the highest priority when multiple declarations are in conflict
- But only use !important as a last resource, It's better to use correct specificities - this will make our code more maintainable for the future
- inline styles in html will always have priority over styles in external stylesheets (using inline styles is not a good practice)
- A selector that contains 1 ID is more specific than one with 1000 classes
- A selector that contains 1 class is more specific than one with 1000 elements
- The universal selector '\*' has no specificity value (0,0,0,0) - all other selectors have precedence over it
- It is always best to rely on specificity than on the order of selectors - even if we rearrange all the css code we cant mess up with the styles
- But rely on order when using 3rd party stylesheets - always put your stylesheets(author) at the last so as to override the 3rd party styles if necessary

## Lecture : How CSS is parsed, Part 2: Value Processing

### How CSS values are processed

- Each time we use rem or vh or any relative unit it will be converted to pixels
- Declared value - declared by the author using selectors
- Cascaded value - result obtained after the cascade process
- Specified value - This is the default value of a certain css property, if there is no cascaded value
- Computed value - values with relative units are converted to px units so that they can be inherited but if percentage then nothing is computed(CSS keywords like auto, orange, bolder are computed and replaced in this step)
- Used value - in this step the css engine uses the rendered layout to figure out some of the remaining values - example percentage value that depend on the layouts (parser need to know the parent element width in order to calculate the percentage value )
- Actual value - browser cant really display 184.8px that is way to specific - usually value with decimal point is usually rounded
- Each and every CSS property needs to have a value even if we dont declare it at all
- Each css property has something called initial value which is used when their is no cascaded value - if we didnt declare a value neither the browser or the user declared a value then the initial value is used - for padding the initial value is 0px and each property has different initial values
- root font size - is the overall font-size of the document - the browser usually has the default value of 16px (useragent declaration)
- some property related to text like the font size inherit the computed value from their parent element - defining font size on each and every element is not practical so we can define on parent element so that the children will inherit it unless we override it

### How CSS engine converts relative units to pixels (absolute units)

- relative units are fundamental to modern responsive layout
- Their is a distinction using percentages for fonts and percentages for lengths and distance measurements - eg. 150% font size means the element will have a font-size 150% larger than its parent's computed font-size
- For length expressed in percentages it works bit different - like padding, margin or height are expressed in percentages - reference is always with respect to the parent element's width
- Then we have font based realtive units em and rem - em is different for fonts and for lengths - em use the parent or current element as the reference while rem use the root font size as reference
- 3em for font size is simply the 3\*parent's computed font-size - for length it is different 2em padding on the header - uses the current element's computed font size as a reference
- rem works the same way for both font-sizes and length - because it always uses the root computed font-size as reference
- We have 2 more relative units - vh and vw are based on browser's view port - 1vh is the 1% of the viewport height and 1vw is 1% of viewport width - The browser knows the viewport height and width values so it does this calculations when it calculate the used values in pixels - generally hero sections will be 100vh - 100% of viewport height

> NOTE : By using em and rem we can build more robust responsive layouts - just by changing font-size we can automatically change the length because it depends on the font size - it gives us great flexibility

### Summary:

- Each property has an initial value which is used if nothing is declared and if there is no inherited value
- Browser specify the root/default font-size for each page (usually 16px) - useragent definition not coming from CSS specification
- To enable css engine to render pages on the screen , the percentages and relative values are always converted to pixels
- if percentages are used to specify font-sizes, they are measured in relative to their parent's font size
- if percentages are used to specify lengths, they are measured in relative to their parent's width
- if em are used to specify font-size, they are measured in relative to their parent font-size
- if em are used to specify lengths, they are measured in relative to their current element's font-size
- rem are always measured relative to the document's root font-size
- vh and vw are simply percentage measurements of the viewport's height and width

## Lecture: How CSS is parsed , Part 3 - Inheritance

### Inheritance in CSS

- Inheritance is the way of propagating property values from parent to the children
- Each CSS property must have a value even if we dont specify it
- If their is a cascaded value then it will be used - if their is no cascaded value then css engine will check if the property can be inherited, that depends on each property
- from line-height property specification - we can see that this property is inherited
- if a property is inherited then the value of that property becomes the computed value of the parent element - it is not the declared value that is passed to the child
- if a property is not inherited like padding then the specified value will become the initial value specific to each property

### Summary

- Inheritance passes the values for some specific properties from parent to children which results in more maintainable code and allows to write less code
- In the property specification for each and every CSS property we can find whether the property is automatically inherited or not
- As a rule of thumb the properties related to text are inherited like font-family, font-size, color etc. (margins and padding are not inherited)
- What gets inherited is the computed value of a property and not the declared value
- Inheritance of a property only works if no one(either the developer or the author) declares a value for that property
- We can use 'inherit' keyword to force inheritance on a certain property and 'initial' keyword can be used to reset a property to its initial value.

## Lecture : Converting px to rem - An effective workflow

- with just changing the root font-size we can adjust all the relative measurements making the site more responsive and robust

## Lecture : How CSS renders a website : The visual formatting model

### Visual Formatting Model

- Is an algorithm that calculates boxes and determines the layout of these boxes, for each element in the render tree in order to determine the final layout of the page
- To calculate the algorithm uses the dimensions of the boxes calculated from box-model, box type like inline, block or inline block, positioning scheme - floats and positioning, stacking contexts, other elements in the render tree such as siblings or parent and viewport size and dimensions of the images etc

### Box model

- is one of the factor that determines how elements are displayed on a web page and how they are sized
- according to this model each and every element can be seen as a rectangular box - each box can have a height, width, padding, margin and border(but they are optional)
- Content area is where all the text and images and other contents that we define go - we can define its height and width using the corresponding CSS property
- Padding is a transparent area around the content, but inside of the box - we use padding to generate white space inside of the box
- Border goes around the padding and the content
- Margin is a transparent area between boxes
- Fill area is an area that gets filled with background color and background image (these properties are applied to entire fill area not only to the content)- that includes content, padding and border

> Default box model - total width = right border+ right padding+specified width+left padding +left border
>
> total height = top border+ top padding+specified height+bottom border+bottom padding

> whenever we define the height of the box the padding and border will get added to it - solution is to use box-sizing property with the value of border-box
>
> height and width will be defined for the entire box including padding and border and not just content area - so total width is the specified width and total height is the specified height - this applies mainly to block level boxes

#### Block level boxes

- type of the box is always determined by display property
  - display block, display flex, display table, display list-item all produces block level boxes
- This block always occupies as much space as possible - usually 100% of parent's width - creates line breaks after and before it meaning blocks are formatted vertically one after the another

#### Inline boxes

- Only occupies the space that its content actually needs - so content distributed in lines
- no line breaks added - they sit inside the block level parent element
- height and width property do not apply
- we can only specify horizontal paddings and margins on these elements(left and right)

#### Inline-block boxes

- Technically also inline boxes but simply works as the block level box on the inside
- they use up only their content space and add no line breaks
- But since they work as block level box on the inside box model applies to them just like regular block level boxes

### Positioning schemes: Normal flow, absolute positioning and floats

#### Normal flow

- Happens to an element if we dont specify a positioning scheme - default positioning scheme
- Not floated and not absolutely positioned - if we use position relative then the element is still in the normal flow
- Elements are simply laid out on the page according to their natural order in the code

#### Floats

- float property causes an element to be completely taken out of the normal flow and shift it to left or right as far as possible until it touches the edge of the containing box or another float element
- When this happens the text and inline elements wrap around the floated elements
- The container will not adjust its height to this floated element - the solution to this is to use clear fixes

#### Absolute Positioning

- when we set the position property to absolute or fixed the element is taken out of the normal flow
- But in absolute positioning the element has no impact on the surrounding content or elements at all, we can even overlap them
- We use css properties top, bottom, left and right to offset the element from its relatively positioned container

### Stacking contexts

- stacking contexts determines in which order the elements are rendered on the page, a new stacking context can be created by different css properties but most widely used is the z-index, but their are other properties as well
- stacking contexts alike layers to form a stack- layers on the bottom of the stack are painted first and elements higher up the stack appear on the top
- The one with higher z index appears on top and one with lower z index appears on bottom
- opacity, transform, filters also create new stacking contexts

## Lecture : CSS Architecture, Components and BEM

### Think, Build and Architect Mindset

- we want clean, modular, reusable and easy to add more features (scalable and maintainable code) for our web page to grow
- We need to think about the layout of our webpage or web app before writing the code
- Build our layout in HTML and CSS with a consistent structure for naming our classes
- We should also create a logical CSS architecture with multiple files and folders

#### Think

- Component-driven design - principle used across all software developments - with component driven design we try to divide our page into modular components
- components are building blocks that we use to construct our interfaces
- we can basically think of our interface as a collection of components held together by the overall layout of the page
- components must be reusable across a project and between different projects
- components should be independent(should not depend on their parent element), so that we can use them completely on their own - allowing us to use them anywhere on the page
- this makes the code reusable and maintainable - these are general rules

#### Build

- after thinking about the design we need to code them in html and css - we should have a consistent structure for naming our classes
- BEM - Block Element Modifier - nice clean system for marking up our layouts
- In BEM, a block is a standalone component(that can be reused anywhere in the project - like btn and recipe) that is meaningful on its own - components are blocks in BEM
- Element is a part of a block that has no meaning on its own - this creates low specificity BEM selectors because we always use classes that are not nested - so this is one of the main reason why BEM is used to write maintainable code
- Modifier is a flag that we can put on a block or element in order to make a different version of the regular block or element
- We can simply know how all elements are related and what each of does by using the BEM structure

#### Architect

- Creating logical folder and file structure for our css to live in
- 7-1 pattern - is simple all this means is that we have 7 different folders for partial Sass(any other CSS Preprocessors) files and 1 main Sass file to import all other partial files into a compiled CSS stylesheet
- The 7 folders are base/(basic project definitions), components/(one file for each component), layout/(define overall layout of the project), pages/(styles for specific pages of the project), themes/(if we want to implement different visual themes), abstracts/(where we put code that doesnt output any CSS such as variables) and vendors/(where all third party css goes)
- we dont need to use all of these folders it depends on the size and scope of the project

### Lecture: What is Sass

- Sass is a CSS preprocessor an extension of CSS that adds power and elegance to the basic language
- We use Sass to fix the problems that we have with CSS - big CSS files becomes completely unmanageble after sometime - that where we use Sass - provides us with some handy tools and features that css doesnt have at the same time doesn't change the fundamental way in which the CSS works
- Instead of writing CSS files we write Sass code in Sass files and we run a compiler which converts the Sass code into regular CSS code - we need to process Sass code first to generate CSS hence it is called CSS Preprocessor

#### Main Sass Features

- Variables - allows us to have reusable values such as colors, font-sizes and spacing etc
- Nesting - to nest selectors inside of one another, allowing us to write less code
- Operators - to perform mathematical operations right inside the CSS
- Partials and imports - important feature of Sass that allows us to write CSS in different files and importing them all into one single file
- Mixins - to write reusable pieces of CSS code
- Functions - similar to mixins, with the difference that they produce a value that can be used later
- Extends - to make different selectors inherit declarations that are common to all of them
- Control directives - for writing complex code using conditionals and loops
- It has two syntax - Sass syntax and SCSS(Sass CSS) syntax that exactly looks like CSS code and helps easy convertion from CSS to Sass code

#### Frist steps with Sass: Mixins, Extends and Functions

- Mixins is a reusable code that we write into a mixin and whenever we use that mixin that code is put in the place where we called the mixin - like a variable with multiple lines of code

### Lecture : To install Sass locally

- `npm init` -- to first bring the package.json file into current project
- `npm install node-sass --save-dev` - to install node sass package as a developer dependency which is a tool that helps in development of project
- `npm install jquery --save` - it is not a tool for development but it is a JS file that we need to include in the project
- `npm install` - to regenerate node_modules (to install all package dependencies)
- `npm uninstall jquery --save` - to uninstall and remove dependency from package.json
- `npm run compile:sass` - to compile the input scss file and output it to specified output file - can also add '-w' flag to watch the changes to scss file
- `npm install live-server -g` - global installation to be used by all projects not just one project - it will not fall under package.json dependencies or be in the node_modules folder

### Lecture : Implementing the 7 - 1 CSS Architecture with Sass

- partial scss file starts with \_ which can be imported into the main scss file
- the base folder contains the basic boiler plate template styles
- abstracts folder contains the code that doesnt output any CSS like variables, mixins and functions
- components folder - we create one file for each components - components are reusable building blocks that make up our websites -completely independent
- layout - components are held together by the layouts of the page - for each piece of the global layout of the entire project - global footer and global header etc which can be used on all pages
- pages folder - when we have specific styles for specific page(styles specific to landing page) - we then create new file for that page
- there can be a folder for themes - if we are creating a web app with different themes
- there can also be a vendors folder - where we can include 3rd party css - like css file of a bootstrap or an icon system

### Lecture : Responsive Design Principles and Layout Types

- Responsive web design is a technique to make the web page adjust its layout or visual style to any possible screen size / also to browser viewports

#### 4 responsive web design principles

#### Fluid layouts

---

- Basically allow web page to adapt to current viewport width (or even height)
- we can simply use the percentage(%,vh,vw) unit instead of pixel unit for all the elements that should adapt to the viewport
- use max-width property instead of simply width
- We can build fluid layouts using float layouts, flexbox and CSS Grid
- Float layouts - are old way of building layouts of all sizes using the float CSS property
- Flex box - Modern way of laying out elements in 1 dimensional row without using floats, perfect for components layout
- CSS Grid - We can build complete 2-dimensional grid layouts - perfect for page layouts and complex components

#### Responsive units

---

- Use rem unit instead of px for most of the length that we define in our CSS
- This will make it easy to scale the entire page layout up or down automatically

#### Flexible/Fluid Images

---

- By default images behave very differently than text content because they dont change automatically as we change the viewport, so we need to fix that
- Usually we make images flexible by defining their dimensions in %, together with the max-width property

#### Media Queries

---

- Is the main principle that brings all the other principles together
- It allows us to change the CSS styles on certain viewport widths called breakpoints
- It allows us to create different versions of the website for different devices

### Lecture : Building a custom grid with floats

- Grid is designed to build consistent interfaces - columns will have a container and it is called row - 2 dimensional grid
- Emmet extension allows to write html faster

### Lecture : Responsive design stratergies:

- max-width for desktop first and min-width for mobile first approach

#### Desktop first

- max-width media queries - checks if the current viewport width is smaller or equal to 600px - if yes then these media query will apply - it is the maximum width at which the media queries still works after that width it is not applied
- media queries dont add any importance or specificity to selectors, so code order matters - media query at the end will take precedence

#### Mobile first

- min-width media queries - checks if the current viewport width is larger or equal to 600px if so then the media queries applies - this is the minimum width at which the media queries starts to apply
- whole stratergy is to narrow down our interfaces to bare minimum, to build fast and 100% optimised product for mobile users
- prioritize content over aesthetic design - content is more important

#### Breakpoints

- are viewport width at which our designs need to change - 3 ways to choose them
- **BAD** - simply using the width of popular devices as the breakpoints
- **GOOD** - group most used devices in internet and try to group them in a logical way and decide the breakpoint from them
- **PERFECT** - Look at the content in your design, width are decided based on where the design content breaks
