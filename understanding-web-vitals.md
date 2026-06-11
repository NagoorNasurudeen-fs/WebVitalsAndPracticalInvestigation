# Web Vitals – Concept + Practical Investigation

## Part 1 – Concept Understanding

### What are Web Vitals vs Core Web Vitals

*Web vitals* are a broad set of metrics that measures the user experience using FCP, LCP, TTFB, INP , CLS etc. It is used for general performance monitoring. 

*Core Web Vitals* are a subset of web vitals that are key indicators of page experience and search engine ranking. It includes LCP (Largest contentful paint) , INP (Interaction to Next Paint) and CLS (Cumulative Layout Shift). These metrics reflects loading speed, responsiveness and visual stabilty respectfully. 

###     

The time form when the user starts loading the webpage until the first piece of the content is rendered. 

Good threshold : less than 1.8 seconds

#### Commom causes of poor FCP

1. Slow server response (TTFB)
2. Render Bloacking CSS.
3. Large js bundles
4. Slow network connection
5. Excessive redirects

### LCP (Largest Contentful Paint)

It is one of the three core web vital which tells how long the largest visual element in the user viewport renders completely.It could be <img> ,<image> inside a svg,a backgorund image laoded via url or <video> etc. 

Good threshold : 2.5 seconds.

#### commom causes of poor LCP

1. Hight TTFB
2. Slow network
3. render blocking javascript
4. Resource delay
5. Uncompressed Image is used

### TBT (Total Blocking Time)

It is one of the most important metric in accessing webpage responsiveness. It is total amount of time between First contentful paint and TTi (Time T interactive). It is the time where the browser cannot paint and respond to the user inputs.

Good threshold : 0 - 100ms

#### commom causes of poor TBT

1. Running large task in the browser;s single thread.
2. Monolithic heavy  js bundles.
3. Reduntant an third part libraries
4. Complex layout calculations

### CLS (Cumulative Layout Shift)

It is third pillar in the core web vitals which focus on visual stabilty . It addressses the most frustrating issue i.e. the unexpected movement of an element while reading or interacting with it. CLS measures the sum total of all unexpected layout shifts that occur during the entire lifespan of a page.A layout shift happens at any time a visible elements change its position in one rendered frame to the next. 

It is calculated as Impact fraction * Distance fraction.

Impact fraction : How much space the unstable element takes up in the viewport between two frames. 
Distance fraction : How far the unstable element moved in the viewport.

Good threshold : 0-0.1

#### commom causes of poor CLS

1. Not mentioning height and width in the element
2. Dynamically injected contents is not configurd early
3. Flash of unstyled text

### SI (Speed Index)

It measures how quickly the contents of the page is visually populated during its loading sequence.It captures frame by frame loading behavior. For example consider two site if site 1 loads progressively and finish loading in its 4th second and site 2 show blank screen for 3.8 seconds and suddenly loads and completes loading in its 4th second, then we say site 1 has high speed index but site 2 is less since it shows black screen.

Good threshold : 3.4 seconds

#### commom causes of poor Si

1. Uncompressed image
2. Render blocking css
3. Heavy js parsing due to frameworks

### Lab data (Lighthouse) vs Field data (CrUX/RUM) 

Lighthouse : It uses artificial synthetic environment to analyse the webite ny simulating networks ,throttling of a mid tier device. It suggests tips and provides diagnostic to level the overall score of the website. But it cannot reflect real user behavior.

Field data : These are real data collected from real users using CrUX (Chrome User Experirence) or by ourself from RUM(Real User Monitoring). It captures real user behavior and spots exact problems and bottlenecks.

These two source may differ in data due to device discrepency and unpredictable network conditions etc.

## Part 2 – Practical Investigation

### Metrics Table

| Site | Device | FCP | LCP | TBT | CLS |SI | PerformanceScore | Accessibility | Best Practices | SEO |
|---|---|---|---|---|---|---|---|---|---|---|
|bbc.com|desktop|0.7|1.1|2220|0.016|3.0|60|100|81|100|
|bbc.com|mobile|2.3|5.8|15150|0.017|9.5|37|95|81|100|
|amazon.in|destop|0.9|1.1|340|0|3.5|73|81|58|90|
|amazon.in|mobile|5.3|5.5|2280|0.116|8.7|30|96|54|92|


### Top issues

#### For desktop
1. Reduce js execution time
2. minimize main thread work
3. Page prevented back/forward cache

#### For mobile

1. Avoid enormous workloads
2. Reduce unused css
3. Reduce js execution time

### Root-Cause Notes
1. Blocking JavaScript increasing TBT
2. Diemensions are not explicitly given increasing CLS
3. Large  image delaying LCP
4. Render blocking increases FCP
5. Network dependency tree increases TBT
6. Blocking JavaScript increasing SI

### Prioritized recommendations

1. LCP: compress/resize hero image, serve WebP/AVIF, add preload for hero image
2. TBT: defer non-critical JS, split long tasks, load third-party scripts asynchronously
3. CLS: reserve image dimensions, use font-display: swap, pre-allocate ad slots

### The most impactful first recommendation

1. TBT: defer non-critical JS, split long tasks, load third-party scripts asynchronously