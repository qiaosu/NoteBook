Ruby设计模式笔记
=======

Note for \<Design Patterns in Ruby\>

###<div style="border-top: dotted 1px #ccc;margin-top:2em;">Patterns</div>

<br>

`meta-design patterns`

1. Separate out the things that change from those that stay the same.
2. Program to an interface, not an implementation.
3. Prefer composition over inheritance.
4. Delegate, delegate, delegate.

5. You ain’t gonna need it.

###<div style="border-top: dotted 1px #ccc;margin-top:2em;">Patterns in Ruby</div>

<br>

`Template Method Pattern`

目标：输出一份html格式的月报

	class Report
	  def initialize
        @title = 'Monthly Report'
        @text = [ 'Things are going', 'really, really well.' ]     
      end
      def output_report 
        puts('<html>') 
        puts(' <head>') 
        puts(" <title>#{@title}</title>")
        puts(' </head>') 
        puts(' <body>') 
        @text.each do |line|
          puts("	<p>#{line}</p>" ) 
        end
        puts(' </body>')
        puts('</html>') 
      end
    end

需求变化：需要有不同的格式要求。

	class Report
      def initialize
	    @title = 'Monthly Report'
		@text = ['Things are going', 'really, really well.']
	  end

	  def output_report
		output_start
		output_head
		@text.each do |line|
		  output_line(line)
		end
		output_end
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

	  …
	end

	class PlainTextReport < Report
      def output_start
	  end

	  ...
	end

类图：

![Class Diagram for the Template Method pattern](https://i.alipayobjects.com/e/201211/1dzk7fSyKT.png)

<br>

`Strategy Pattern`

上面模板方法的方式实际上是通过继承来实现定制不同的子类。基于继承的实现缺乏运行时改变的灵活性，因此我们更喜欢通过组合的方式来代替继承。

	class Report 
	  attr_reader :title, :text 
	  attr_accessor :formatter
	  def initialize(formatter) 
	    @title = 'Monthly Report' 
	    @text = ['Things are going', 'really, really well.'] 
	    @formatter = formatter
	  end
	  def output_report() 
	    @formatter.output_report(self)
	  end 
	end

	class HTMLFormatter 
	  def output_report(context)
	    puts('<html>') 
	    puts(' <head>') # Output The rest of the report ...
	    puts(" <title>#{context.title}</title>") 
	    puts(' </head>') 
	    puts(' <body>') 
	    context.text.each do |line|
	      puts(" <p>#{line}</p>") 
	    end
	    puts(' </body>')
	    puts('</html>') 
	  end
	end

	class PlainTextFormatter 
	  def output_report(context)
	    puts("***** #{context.title} *****") 
	    context.text.each do |line|
	      puts(line) 
	    end
	  end 
	end

类图：

![The Strategy pattern](https://i.alipayobjects.com/e/201211/1e05hvANTB.png)

<br>

`Observer Pattern`

	module Subject 
	  def initialize
	    @observers=[] 
	  end

	  def add_observer(observer) 
	    @observers << observer
	  end

	  def delete_observer(observer) 
	    @observers.delete(observer)
	  end

	  def notify_observers 
	    @observers.each do |observer|
	      observer.update(self) 
	    end
	  end 
	end

	class Employee
	  include Subject

	  attr_reader :name, :address 
	  attr_reader :salary

	  def initialize( name, title, salary) 
	    super() 
	    @name = name 
	    @title = title
	    @salary = salary 
	  end

	  def salary=(new_salary) 
	    @salary = new_salary 
	    notify_observers
	  end 
	end

类图：

![The Observer pattern](https://i.alipayobjects.com/e/201211/1eIygKWRQ5.png)
