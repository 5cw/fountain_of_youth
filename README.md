
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
- As close to compatible with vanilla Fountain as possible.
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


- Special Pages (exclusively key: value pairs,)
  - Title Page
  - Character Page
  - Outline Page
  - Header and Footer
- Script Body
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

## Shots

Any line in uppercase with a blank line before and after is a Shot. Force a Shot with `$`. A Transition is just a right justified Shot. See [Justification](#justification). (`Transition` formatting applies to any right justified Shot.)

Anything matching Transition criteria is automatically right justified, but notably, `>` no longer forces uppercase. When converting from `.fountain` to `.foy`, either replace `>` with `>$` or capitalize the line.

## Block

Surround any sequence of elements with `{` and `}` to modify them together. A solitary element counts as a block on its own.

## Dialogue Blocks

Any Character, Parentheses and Dialogue elements can be modified together as they automatically form blocks.

## Multi Combine

In FoY, any block may be combined next to any other using the Dual Dialogue Syntax. 

See [examples.foy](examples.foy)

## Modifiers

### Justification

Centered Text and Transitions are genericized so that any element may have its justification changed.

- `<left justified`
- `>centered<` or `><centered`


### Horizontal and Vertical Shifting

If you want to move your text a certain distance from either side, or a certain distance up and down prepend a justification with

- `<<x` to shift left
- `>>x` to shift right
- `><x` to shift left side right and right side left
- `<>x` to shift left side left and right side right
- `<<<x` to shift both sides left
- `>>>x` to shift both sides right
- `^^x` to shift up/down.

where `x` is a [CSS length](https://developer.mozilla.org/en-US/docs/Web/CSS/length). No units will default to `em` in the horizontal case, which is the width of a character, and `lh` in the vertical case, which is the height of a line. If `x` is whitespace (including just a space) it will be interpreted as 1. No gap may lead to ambiguity, and will cause formatting to be interpreted as part of the line.

If shifting, justification must be specified. `>>xtext` or `>>x text` will be parsed as right justified `>xtext` or `x text`.

For formatting, the parser looks for the rightmost justification string `>` or `<`.

- If it's `<` at the end of the string, mark the text as potentially centered, then find a corresponding `>`. If there is none, leave the line unformatted.
- If it's not at the end of the string, mark the text as potentially left or right justified.
- If a justification symbol was found, 


If there's just one horizontal shift, it applies to the justified side.

- `>>2<text` means align the left side of the text 2 characters to the right from where it was originally going to be and leave right margin default.
- `>>2>text<` means align the center of the text 2 characters to the right from where it was originally going to be and shift whichever margin is further away to match the other margin.
- `>>2<text` means align the right side of the text 2 characters to the right from where it was originally going to be and leave left margin default.

If there's two horizontal shifts, they apply to the left side, then the right side.

- `>>2<<3<text` means align the left side of the text 2 characters to the right, and the right margin 3 characters to the left.
- `><2<text` is an alias of `>>2<<2<text`.
- `<>2<text` is an alias of `<<2>>2<text`.
- `>>>2<text` is an alias of `>>2>>2<text`.
- `<<<2<text` is an alias of `<<2<<2<text`.

If there's two horizontal shifts and the text is center aligned, we still align to the left then the right, and put the center in the middle of the two.

If there's three horizontal shifts, then the first one applies to the justified side, the second to the left, the third to the right. This is only necessary for centered text.



- `^^2<text` will shift 2 lines down.
- `^^ <text` will shift 1 line down, effectively adding a carriage return.
- `^^-3<text` will shift 3 lines up, a move which will usually overlap text. 



Replace `x` with `|x|` to measure from the edge of the page and not the original value. This will also make `%` and units like `vh` and `vw` size relative to the entire page and not the bounding box. Left shifts will measure starting at the right edge of the page.

Replace `x` with `[x]` to measure from the current justification. This is useful if you want to offset t
  
### Bulk Styler

The syntax for lyrics `~` becomes a bulk styler,

 - `~` or `~*` for italics 
 - `~**` for bold
 - `~***` for bold italics
 - `~_` for underline


Can be used with Horizontal and Vertical shifting.

This will modify the whole block which comes after it.

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