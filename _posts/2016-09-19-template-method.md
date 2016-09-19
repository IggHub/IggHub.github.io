---
layout:   post
title:    Ruby's template method
tags:     [ruby, ruby design patterns]
comments: true
---


I am relatively new to Ruby since [Bloc](http://bloc.io/) introduced it to me in December last year.

I did not come from a CS background. To supplement my lack of design patterns knowledge, I decided to read some programming books, one of them being [Design Patterns](https://www.amazon.com/Design-Patterns-Ruby-Russ-Olsen/dp/0321490452) book. *(If you don't have much CS background and have been playing with ruby for half a year, I would highly recommend this book because it is relatively much easier to read than others I have read so far)*

The first pattern that I was introduced is called *Template Method*.

## Original Scenario

Let's say your boss ask you to create a code that generate simple HTML report. Then he asked to generate simple plain text report. You came up with something like the one below:

```
class Report

	def initialize
		@title = 'Weekly Report'
		@text = [ 'Things are not going', 'well at all.' ]
	end

	def output_report(format)
		if format == :plain
			puts("*** #{@title} ***")
		elsif format == :html
			puts('<html>')
			puts(' <head>')
			puts(" <title>#{@title}</title>")
			puts(' </head>')
			puts(' <body>')
		else
			raise "Unknown format: #{format}"
		end

		@text.each do |line|

			if format == :plain
				puts(line)
			else
				puts(" <p>#{line}</p>" )
			end
		end

		if format == :html
			puts(' </body>')
			puts('</html>')
		end
	end
end
```

However, each time you added new output format, `Report` class will get super crowded, real fast. If there are ten different formats, just imagine how long `output_report` will be.

## Solution: Template method

Using template method, you break will break down the Classes depending on how many different formats are there.

```
class Report
	def initialize
		@title = 'Monthly Report'
		@text = ['Things are', 'super awesome!']
	end

	def output_report
		output_start
		output_head
		output_body_start
		output_body
		output_body_end
		output_end
	end

	def output_body
		@text.each do |line|
			output_line(line)
		end
	end

	def output_start
	end

	def output_head
		output_line(@title)
	end

	def output_body_start

	end

	def output_line(line)
		raise 'Called abstract method: output_line'
	end

	def output_body_end

	end

	def output_end

	end
end

class HTMLReport < Report
	def output_start
		puts('<html>')
	end

	def output_head
		puts('	<head>')
		puts("	<title>#{@title}</title>")
		puts('	</head>')
	end

	def output_body_start
		puts('<body>')
	end

	def output_line(line)
		puts("	<p>#{line}</p>")#need double quote because of <p>
	end

	def output_body_end
		puts('</body>')
	end

	def output_end
		puts("</html>")
	end
end

class PlainTextReport < Report

	def output_head
		puts("*****#{@title}*****")
		puts
	end

	def output_line(line)
		puts(line)
	end

end

```

It creates a super class `Report`. For any additional formatting, all you need to do is create an new subclass and make `Report` your superclass. `HTMLReport`, `PlainTextReport`, they all inherit from `Report` class. No need to touch the existing code. Clean. Elegant. Simple.

Sample command:
```
reppie = HTMLReport.new
=> #<HTMLReport:0x0056196cbb2250 @title="Monthly Report", @text=["Things are", "super awesome!"]>

reppie.output_report
<html>
  <head>
    <title>Monthly Report</title>
    </head>
<body>
    <p>Things are</p>
    <p>super awesome!</p>
</body>

</html>
```
