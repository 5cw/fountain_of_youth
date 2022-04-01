
- customizable html/css
- near-superset of fountain
- color
- size/font
- images
- more customizable simultaneity 
  

# Fountian of Youth

Fountain of Youth is a WIP near-superset of the [Fountain](https://fountain.io/syntax) markup language for screenwriting which uses .

Draws from [betterfountain by piersdeseilligny.](https://github.com/piersdeseilligny/betterfountain)

[MIT License](LICENSE.md)

Goals for this project:

- Consistent, unambiguous ruleset.
- Reasonably compatible with vanilla Fountain.
- Greater formatting control and customizability
- Greater range of standard features
- Portable conversion tooling

Features:

- [ ] Full Specification
- [ ] HTML and PDF conversion tools for
  - [ ] Python
  - [ ] TypeScript/Javascript
  - [ ] Rust
  - [ ] C/C++
  - [ ] Java/Kotlin
- [ ] Language Server
- [ ] VSCode Extension

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

 Follow a Character and are surrounded by `(` and `)`.

 ## Dialogue
 
 Any text following a Character or Parenthetical.

 ### Dual Dialogue

 Any character name followed by a caret `^` will combine with the last dialogue side-by-side.

 ## Lyrics

 Any text beginning with `~`.

 ## Transition

 Any uppercase text surrounded by empty lines ending in `TO:` or any text beginning with `>`.

 *Fountain++ makes this a right-justified Shot rather than its own element. See Justification.*

 ## Centered Text

 Any text surrounded by `>` and `<`.

 *Fountain++ makes this a property rather than its own element. See Justification.*

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


# Fountain++ Additions/Changes

Fountain++ reorganizes the elements. These are the kinds of elements which exist:


- Special Pages
  - Title Page Key: Value
- Character Page
  - Character: Value
  - Description: Value
- Outline Page
  - Auto: Value
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

## Shots

Any line in uppercase with a blank line before and after is a Shot. A Transition is just a right justified Shot. (`Transition` formatting applies to any right justified Shot.)

## Dialogue Blocks

Any Character 

## Multi Combine

Dual Dialogue is preserved and extended.

Any line that isn't Dialogue may also be combined horizontally with the same trailing 

## Modifiers

### Justification

Centered Text and Transitions are genericized into `>centered<` and `>right justified` modifiers and are joined by the `<left justified` modifier. `<text>` is reserved for later use. These may be applied to any other element. 

Anything matching Transition criteria is automatically right justified, but notably, `>` no longer forces uppercase. 
  
### Bulk formatter

The lyrics modifier `~` becomes a bulk formatter,

 - `~` or `~*` for italics 
 - `~**` for bold
 - `~***` for bold italics
 - `~_` for underline
 - `~<` for left justified
 - `~>` for right justified.

This will modify a whole line, element, or block.

`Lyrics` formatting applies to any bulk italicized Action or Dialogue.

## Custom Formatting

If you want your text a certain distance from either side, use 

- `<format<text` left justified
- `>format>text<` centered
- `>format>text` right justified
  
where `format` is of the form

- `x`
- `x/x`

where `x` is a [CSS length](https://developer.mozilla.org/en-US/docs/Web/CSS/length). No units will default to `em`.

Replace `x` with `|x|` to measure from the edge of the page and not the edge of the bounding box. This will also 


### Inline css

For font, color and anything else, Fountain++ allows this as an optional way to do formatting, and change the global formatting for a given element.

## Title Page

Added everything betterfountain has. 

- [ ] TODO look up the betterfountain title page parts
- [ ] decide on syntax for inline css

## Errors

### UTF-8
 
 Fountain++ is encoded in UTF-8 and therefore may contain all of unicode. 

 Scripts are most commonly written in the english speaking world to look like they were written on a typewriter, which means no fancy punctuation by default.