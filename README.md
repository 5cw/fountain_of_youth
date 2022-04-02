
- customizable html/css
- near-superset of fountain
- color
- size/font
- images
- more customizable simultaneity 
  

# Fountian of Youth

Fountain of Youth is a WIP near-superset of the [Fountain](https://fountain.io/syntax) markup language for screenwriting which uses `.foy` file extensions.

Draws from [betterfountain by piersdeseilligny.](https://github.com/piersdeseilligny/betterfountain)

[MIT License](LICENSE.md)

Principles for this project:

- Consistent, unambiguous ruleset.
- Close to compatible with vanilla Fountain.
- Greater formatting control and customizability.
- Greater range of standard features.

Goals:

- [ ] Full Specification
- [ ] HTML and PDF conversion tools for
  - [ ] Python
  - [ ] TypeScript/Javascript
  - [ ] Rust
  - [ ] C/C++
  - [ ] Java/Kotlin
  - [ ] Windows command prompt
  - [ ] MacOS and Linux Terminal
- [ ] Tools for converting edge case .fountain files to .foy
- [ ] Language Server
- [ ] VSCode Extension (maybe implement into betterfountain)

# Default Fountain syntax

 ## Title Page

 All optional page parts, with form `key: value`. 

 Default formatting puts keys `Title`, `Credit`, `Source`, and `Author(s)` in the center,  `Contact` and `Draft date` in the lower left.

 ## Sections

 Any line beginning with any number of `#` will be converted to implementation dependent navigable sections in a similar way to markdown.

 ## Synopses

 Any line beginning with `=`. Implementation dependent.

 ## Scene Headings

 `INT EXT EST INT./EXT INT/EXT I/E` or begining with `.`

 ### Scene Numbers

 `#x#` following a Scene Heading where x can be any alphanumerics `(0-9)(a-z)(A-Z)` or `-` or `.`.

 ## Actions

 Any paragraph that does not meet any other criteria for an element or beginning with `!`.

 ## Characters

 Any line entirely in uppercase, with one empty line before it and without an empty line after it or beginning with `@`.

 ## Parenthetical

 Follow a Character or Dialogue and are surrounded by `(` and `)`.

 ## Dialogue
 
 Any text following a Character or Parenthetical.

 ### Dual Dialogue

 Any character name followed by a caret `^` will combine with the last dialogue side-by-side.

 ## Lyrics

 Any text beginning with `~`.

 ## Transition

 Any uppercase text surrounded by empty lines ending in `TO:` or any text beginning with `>`.

 *Fountain of Youth makes this a right-justified Shot rather than its own element. See Justification.*

 ## Centered Text

 Any text surrounded by `>` and `<`.

 *Fountain of Youth makes this a property rather than its own element. See Justification.*

 ## Notes

 Any text surrounded by `[[` and `]]`. Implementation dependent.

 ## Comments (Boneyard)

 Any text surrounded by `/*` and `*/` will be ignored.

 *Any text following `//` on a single line will be ignored.*


 ## Other information

 ### Emphasis

 - `*italics*` 
 - `**bold**` 
 - `***bold italics***` 
 - `_underline_`

 ### Page Breaks

 `===`

 ### Line Breaks and Indentation

 Line breaks will be respected, indentation will be ignored. No formatting matches across line breaks except notes `[[]]` and boneyard `/* */`.


# Fountain of Youth Additions/Changes

Fountain of Youth reorganizes the elements. These are the kinds of elements which exist:


- Special Pages
  - Title Page
  - Character Page
  - Outline Page
- Script Body
  - Header: Value 
  - Footer: Value
  - Dialogue Block
    - Character
    - Parenthetical
    - Dialogue
  - Block
  - Section
  - Synopsis
  - Action
  - Shot
  - Scene Heading
    - Scene Number
  - Notes
  - Comments

## Block

Surround any sequence of elements with `{` and `}` to modify them together

## Dialogue Blocks

Any Character, Parentheses and Dialogue elements can be modified together as they automatically form blocks.

## Shots

Any line in uppercase with a blank line before and after is a Shot. Force a Shot with `$`. A Transition is just a right justified Shot. (`Transition` formatting applies to any right justified Shot.)

## Multi Combine

In FoY, 

## Modifiers

### Justification

Centered Text and Transitions are genericized into `>centered<` and `>right justified` modifiers and are joined by the `<left justified` modifier. These may be applied to any other element. 

Anything matching Transition criteria is automatically right justified, but notably, `>` no longer forces uppercase. 


### Horizontal Shifting

If you want to move your text a certain distance from either side, prepend a justification with

- `<<x` to shift left
- `>>x` to shift right
- `><x` to shift left side right and right side left
- `<>x` to shift left side left and right side right
- `<<<x` to shift both sides left
- `>>>x` to shift both sides right

where `x` is a [CSS length](https://developer.mozilla.org/en-US/docs/Web/CSS/length). No units will default to `em`, which is the width of a character.

If there's just one shift, it applies to the justified side.

- `>>2<text` means align the left side of the text 2 characters to the right from where it was originally going to be and leave right margin default.
- `>>2>text<` means align the center of the text 2 characters to the right from where it was originally going to be and leave margins default.
- `>>2<text` means align the right side of the text 2 characters to the right from where it was originally going to be and leave left margin default.

If there's two shifts, they apply to the left side, then the right side.

- `>>2<<3<text` means align the left side of the text 2 characters to the right, and the right margin 3 characters to the left.

If there's two shifts and the text is center aligned, we still align to the left then the right, but 

- `

Replace `x` with `|x|` to measure from the edge of the page and not the edge of the bounding box. This will also make `%` and units like `vh` and `vw` size relative to the entire page.
  
### Bulk Formatter

The syntax for lyrics `~` becomes a bulk formatter,

 - `~` or `~*` for italics 
 - `~**` for bold
 - `~***` for bold italics
 - `~_` for underline
 - `~<` for left justified
 - `~>` for right justified
 - `~><` for centered

Can be used with Horizontal and Vertical shifting.

This will modify the whole line, element, or block which comes after it.

`Lyrics` formatting applies to any bulk italicized Action or Dialogue.

### Global Formatter






### Inline/External CSS

For font, color and anything else, Fountain of Youth allows CSS as an optional way to do formatting, and change the global formatting for a given element.

## Title Page

Added everything betterfountain has. 

- [ ] TODO look up the betterfountain title page parts
- [ ] decide on syntax for inline css

## Errors

### UTF-8
 
 Fountain of Youth is encoded in UTF-8 and therefore may contain all of unicode. 

 Scripts are most commonly written in the english speaking world to look like they were written on a typewriter, which means no fancy punctuation by default.