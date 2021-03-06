# list-gen

A simple helper package for generating lists in JIMP.

## Basic Usage

```js
const lines = ['Item #1', 'Item #2', '...'];

const options = {
	/* Initial XY Coordinates - Optional, indicates where the first line of text will be placed on the page */
	initialXY: [
		[50, 100], // Page 1
		[50, 20] // Page >= 2 (Optional)
	],
	/* Line Spacing - Optional, a function which modifies x, y values passed through it */
	spacing: (x, y) => {
		return [x, y + 40]; // This example adds 40px to the y value every new line
	},
	/* Write Image - Optional, when specified, outputs .jpg files in addition to returning buffer(s) */
	write: './output/',
	/* Max Lines - Optional, forces a new page when X amount of lines are on the page */
	maxLines: [
		25, // Page 1
		40 // Page >= 2 (Optional)
	],
	/* First Background - Required, the image background displayed on the first (or subsequent) page */
	firstBG: './bg-1.png',
	/* Extra Backgrounds - Optional, the image background displayed on pages 2 and above */
	extraBG: './bg-2.png',
	/* Font - Optional, the font to be used, has to be compatible with Jimp */
	font: './myFont.fnt'
};
```

```js
const List = require('list-image-gen'); // Require Package

const list = new List(lines, options); // Create List Object
const resp = await list.generate(); // Generate list
```

## Examples

_Use `yarn test` or `npm run test` to compile these yourself_

**1. Basic todo list** | [Output](https://github.com/lorencerri/list-image-gen/tree/master/test/output/todolist)

```js
const list = new List(
	[...new Array(22)].map((_, i) => `Item #${i}`),
	{
		initialXY: [420, 420],
		spacing: (x, y) => [x, y + 96.3],
		firstBG: path.join(__dirname, '/resources/todolist.jpg'),
		write: path.join(__dirname, '/output/todolist/'),
		font: Jimp.FONT_SANS_128_BLACK
	}
);
await list.generate();
```

**2. Pixel Art Theme** | [Output](https://github.com/lorencerri/list-image-gen/tree/master/test/output/pixelart)

```js
const list = new List(
	[...new Array(200)].map((_, i) => `Item #${i}`),
	{
		initialXY: [
			[60, 280], // Page 1
			[60, 10] // Page >= 2
		],
		spacing: (x, y, {remainingLines, pageNumber}) => {
			if (pageNumber === 0) {
				y += remainingLines.length % 6 === 0 ? 2 : 0; // Every 6 items on the first page, add 2 to the y position
			}

			return [x, y + 40];
		},
		maxLines: [41, 48],
		write: path.join(__dirname, '/output/pixelart/'),
		firstBG: path.join(__dirname, '/resources/first.png'),
		extraBG: path.join(__dirname, '/resources/extra.png'),
		font: path.join(__dirname, '/resources/hypnoverse.fnt')
	}
);
await list.generate();
```

**3. Staircase Style** | [Output](https://github.com/lorencerri/list-image-gen/tree/master/test/output/staircase)

```js
const list = new List(
	[...new Array(10)].map((_, i) => `User ${i}`),
	{
		initialXY: [0, 5],
		spacing: (x, y) => [x + 100, y + 25],
		write: path.join(__dirname, '/output/staircase/'),
		firstBG: path.join(__dirname, '/resources/background.png')
	}
);
await list.generate();
```