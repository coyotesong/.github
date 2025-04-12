# Oddball Project: paper

## Summary

Create apps that rescale PDF document to optimal scaling to smaller page sizes.
Ideal case is creating new CUPS filters, but even a standalone app would be
useful.

## Business Case

### Primary case: handling small paper sizes

Duplex (2-sided) consumer laser printers are now affordable. Even color laser
printers are not outrageously expensive when you consider that a set of toner
cartridges may last for more than a year. (A full CYMK toner set will be very
expensive but still inexpensive compared to the cost of inkjet cartridges over
the same period, esp. if the printer is often idle for a week or longer.)

They handle standard business paper sizes - A4 and US Letter - without a problem.
They can usually handle the half-sizes as well.(\*) They often struggle with
smaller paper sizes.

This is important since the standard paper sizes are ideal for many uses - but
in practice smaller formats are often better choices since they're much more
portable, esp. the smallest sizes, yet hold enough information on a single
page to be useful. Think of the popularity of "Daytimers" when they were popular,
and remember that we still have the need for offline access to some information
since we may not always have internet service and/or usable devices.

We can duplex print individual sheets on standard paper by specifying suitably
large margins and then cutting it down to the desired size. However this wastes
a lot of paper.

A better choice is a filter that performs an intelligent mapping from one paper
size to another paper size - including a consideration of which page should
appear on the back side of each sheet.

Similar filters could handle small booklets (folios?) that should be folded
and stapled, or the mini-booklet created by cutting a slit in the middle
of the sheet and creative folding.

### Secondary case: templates and forms

There are numerous document templates available that we fill in by hand.
Everything from calendars to Cornell study sheets to workout and dietary
logs, etc.  These can be very useful in ensuring we write down everything
we need to remember, but can also be a pain if they're something we want
to retain for a long period but only have a copy with messy handwriting.

PDFs support forms - you can either fill in the fields immediately or
copy them from handwritten notes later. Adobe Reader won't let you save
these documents (although you can print them), however other tools will
let you save them.

Any serious word editor can also support PDF forms.

The secondary case is being able to create basic templates programmatically -
without using a word processor.

The original motivation was coming across some old graph paper - from
the pre-computer age where we had to purchase it from the university
book store for ten cents per sheet - and thinking about how nice it
would have been to be able to both print out our own graph paper AND to
have the software use standard PostScript commands to actually draw
the graphs for us. I'm sure many applications can do the same thing today
but it got me thinking about the options.

For normal templates, e.g., Cornell notes, the word processor approach
is still the easist approach but a programmatic approach may be better
if we want to match exact dimensions with an existing document or if
we want it to be "alive" using PDF's javascript support.

## Current status

This is another "I'm using java to start since I'm most familiar with
it but know I should change languages once I've nailed down the requirements"
project.

The current status is POC, barely. I can use my browser to "print to PDF"
and then scale the results to a variety of paper sizes with a proper
alignment between front and back page. This is for both single sheets
and folios of multiple pages.

However it's still a multi-step process since I must "print to PDF",
copy the file to a specific location, run the application, then send
the modified document to the printer. In theory I should be able to
just pipe it to `lpr` since my printer supports PostScript but (iirc)
I still need to load the results into a PDF viewer and then print it.

So it works but has too many rough edges at the moment.
