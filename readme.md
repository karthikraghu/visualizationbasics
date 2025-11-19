Programming Exercise 1

1.2 Added title to the Visualization in the Index.html

-----------------------------------------

1.3 Data Loading

What This Function Does
Think of CSV data coming in like this:

The row function converts this messy text into clean, usable data.

Line by Line Explanation
Line 1: const row = d => {

Creates a function that takes one parameter d (one row from your CSV file)
Line 2: d.coords = d['Location Coordinates'].split(',').map(d => +d).reverse();
Let's break this chain:

d['Location Coordinates'] → Gets "40.7128, -74.0060" (text)
.split(',') → Splits by comma into array: ["40.7128", " -74.0060"]
.map(d => +d) → Converts each string to a number: [40.7128, -74.0060]
The + before d converts string to number
.reverse() → Flips the order: [-74.0060, 40.7128]
Geographic data often needs [longitude, latitude] order for mapping
Line 3: d['Total Dead and Missing'] = +d['Total Dead and Missing'];

Converts "25" (text) → 25 (number)
The + converts string to number so you can do math with it
Line 4: d['Reported Date'] = new Date(d['Reported Date']);

Converts "2023-01-15" (text) → Date object
Date objects let you compare dates, format them, extract year/month, etc.
Line 5: return d;

Gives back the transformed row with all the cleaned-up data


Line 1: const useData = () => {

Creates a custom hook (a reusable function for React components)
Line 2: const [data, setData] = React.useState(null);

Creates a state variable called data that starts as null (empty)
setData is a function to update the data variable
Think of it like a box: data is what's in the box, setData is how you put things in the box
Initially the box is empty (null)
Line 3-6: React.useEffect(() => { ... }, []);
This is the magic part that controls WHEN code runs:

The code inside runs only once when the component first appears on screen
The empty [] at the end means "run once and never again"
Without this, the data would load over and over infinitely!
Line 5: d3.csv(csvUrl, row).then(setData);
Let's trace what happens:

d3.csv(csvUrl, row) - Starts loading the CSV file, using the row function to clean each row
This takes time (like downloading), so it returns a Promise (a promise to give you data later)
.then(setData) - When loading finishes, call setData with the loaded data
setData updates the data variable, which triggers React to re-render
Line 7: return data;

Returns whatever is in the box (first null, then the actual data after loading)

-------------------------------------------

1.4 Added the introduction text with dynamic values

-------------------------------------------

1.5 World Sphere and Graticule


The D3 Setup (Lines 1-5)
Line 1: const projection = d3.geoNaturalEarth1();

Imagine trying to flatten an orange peel - that's what projections do to the round Earth
geoNaturalEarth1 is one way to flatten Earth onto a 2D screen
It takes 3D coordinates (latitude/longitude) and converts them to x,y pixels
Line 2: const path = d3.geoPath(projection);

This is like a drawing pen that knows how to use the projection
You give it geographic data (like "draw a circle around Earth"), and it converts it to SVG path commands
It's a translator: geographic shapes → screen shapes
Line 3: const graticule = d3.geoGraticule();

Creates a grid line generator
Like the latitude/longitude lines you see on a globe
By default creates lines every 10 degrees
The React Component (Lines 7-17)
Line 7: const WorldGraticule = ({width, height}) => (

A React component that receives width and height as props (though not used here)
The ({width, height}) is destructuring - unpacking values from an object
Line 9: <g className="worldGraticule">

<g> is an SVG group element - like a container/folder for related shapes
Groups things together for styling and organization
Line 12: <path className="sphere" d={path({type: 'Sphere'})} />
Let's break this down:

{type: 'Sphere'} → Geographic data meaning "the whole Earth as a circle"
path(...) → Converts that sphere into SVG path data
d={...} → The d attribute is the actual drawing instructions for SVG
Result: Draws a circular outline representing Earth's edge
Line 15: <path className="graticule" d={path(graticule())} />

graticule() → Generates all the lat/long grid lines
path(...) → Converts those lines to SVG format
Result: Draws the grid lines across the sphere
The CSS Styling
Makes the Earth's background almost white (#fbfbfb)
Grid lines are just strokes (outlines), not filled shapes
Very light gray so they don't distract from your data
Visual Result
Imagine you're creating:

A light gray circle (the Earth's background)
Light gray grid lines across it (latitude/longitude)
Everything ready for you to draw countries and data points on top

