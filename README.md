## one-file-pdf - A minimalist PDF generator in &lt;2K lines and 1 file
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![godoc](https://godoc.org/github.com/balacode/one-file-pdf?status.svg)](https://godoc.org/github.com/balacode/one-file-pdf)
[![Go Report Card](https://goreportcard.com/badge/github.com/balacode/one-file-pdf)](https://goreportcard.com/report/github.com/balacode/one-file-pdf)
[![Build Status](https://travis-ci.org/balacode/one-file-pdf.svg?branch=master)](https://travis-ci.org/balacode/one-file-pdf)  

The main idea behind this project was:  
*"How small can I make a PDF generator for it to still be useful for 80% of common PDF generation needs?"*

The result is a single .go file with less than 1999 lines of code.

- All the basics for generating PDF documents, enough for generating mundane business reports.
- It's easier to learn about the internals of the PDF format with a smaller library.
- You can just drop *one_file_pdf.go* into your Go project. No need to manage dependencies. (But remember to change the package to 'main' or whatever package you are adding it to.)
- The current version of the file is indicated in the header (the timestamp).

**Supported Features (the fundamentals):**
- You can use all built-in PDF fonts: Courier, Helvetica, Symbol, Times, ZapfDingbats, and their variants
- Recognises 144 web colo(u)r names, or any RGB value
- Stream compression can be turned on or off (normal PDF files compress streams to reduce file size, but turning compression off helps in debugging or learning about PDF commands)
- Properties to set metadata: author, creator, keywords, subject and title
- Set the measurement units you want: cm, inches, points, etc.
- Draw lines with different thickness
- Draw filled or outline rectangles
- Draw grayscale PNG images
- Set columns for outputting text (like arbitrary tab stops on the page)
- Built-in grid that can be enabled to assist measurement and positioning

**Not Supported (everything else):**
- Unicode (requires font embedding)
- Font embedding
- PDF encryption
- Paths, circles, curves and complex graphics

**Hello World**

```go
package main 

import "fmt" // standard
import "github.com/balacode/one-file-pdf"

func main() {
	fmt.Println(`Generating a "Hello World" PDF...`)
	//
	// create a new PDF using 'A4' page size
	var pdf = pdf.NewPDF("A4")
	//
	// add a page: this must be done before drawing
	pdf.AddPage()
	//
	// set the measurement units to centimeters
	pdf.SetUnits("cm")
	//
	// draw a grid to help us align stuff (just a guide, not necessary)
	pdf.DrawUnitGrid()
	//
	// draw the word 'HELLO' in orange, using 100pt bold Helvetica font
	// - text is placed on top of, not below the Y-coordinate
	// - you can use method chaining
	pdf.SetFont("Helvetica-Bold", 100).
		SetXY(5, 5).
		SetColor("Orange").
		DrawText("HELLO")
	//
	// draw the word 'WORLD' in blue-violet, using 100pt Helvetica font
	// note that here we use the colo(u)r hex code instead
	// of its name, using the CSS/HTML format: #RRGGBB
	pdf.SetXY(5, 9).
		SetColor("#8A2BE2").
		SetFont("Helvetica", 100).
		DrawText("WORLD!")
	//
	// draw a flower icon using 300pt Zapf-Dingbats font
	pdf.SetXY(7, 17).
		SetColor("Red").
		SetFont("ZapfDingbats", 300).
		DrawText("a")
	//
	// save the file:
	// if the file exists, it will be overwritten
	// if the file is in use, prints an error message
	pdf.SaveFile("hello.pdf")
} //                                                                        main
```
