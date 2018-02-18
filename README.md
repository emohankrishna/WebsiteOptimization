## Website Performance Optimization portfolio project

Your challenge, if you wish to accept it (and we sure hope you will), is to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the [Critical Rendering Path course](https://www.udacity.com/course/ud884).

To get started, check out the repository and inspect the code.

Some useful tips to help you get started:

1. Check out the repository
1. To inspect the site on your phone, you can run a local server

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

1. Open a browser and visit localhost:8080
1. Download and install [ngrok](https://ngrok.com/) to the top-level of your project directory to make your local server accessible remotely.
1.This can be done by opening another new terminal 

  ``` bash
  $> cd /path/to/your-project-folder
  $> ./ngrok http 8080
  ```

1. Copy the public URL ngrok gives you and try running it through PageSpeed Insights! Optional: [More on integrating ngrok, Grunt and PageSpeed.](http://www.jamescryer.com/2014/06/12/grunt-pagespeed-and-ngrok-locally-testing/)

Profile, optimize, measure... and then lather, rinse, and repeat. Good luck!
### Getting started

#### Part 1: Optimize PageSpeed Insights score for index.html

1.Reducing the `/views/images/pizzeria.jpg` image size by compressing and resizing to `115*50 px`.
1.By adding async tag to javascript files whichnare referenced in `/index.html`,`/project-2048.html`,`/project-mobile.html`,`/project-webperf.html` files to block critical requests and request asyncrously.
1.To block the critical rendering of CSS files modify the link tag
  ``` bash
  <link rel="preload" href="css/style.css" as="style" onload="this.rel='stylesheet'">
  ```
1.Add the media element for CSS file links to download the file when print is requested.
  ``` bash
  <link href="css/print.css" rel="stylesheet" media="paint"> 
  ```

#### Part 2: Optimize Frames per Second in pizza.html
1.Delete the `/views/css/style.css` file and make internal styles to eleminate render blocking files.
1.Modify the `/views/js/main.js` to eliminate Forced Synchronous Layout for change of pizzas size.

   ``` 
    // Iterates through pizza elements on the page and changes their widths
  function changePizzaSizes(size) {
    var randomPizza=document.querySelectorAll(".randomPizzaContainer");
    switch(size) {
        case "1":
                  var newwidth= 25;
                  break;
        case "2": var newwidth= 33.33;
                  break;
          return ;
        case "3": var newwidth= 50;
                  break;
        default:
          console.log("bug in sizeSwitcher");
      } 
    
    for (var i = 0; i < randomPizza.length; i++) {
      randomPizza[i].style.width = newwidth+'%';
    }
  }
  changePizzaSizes(size);
  ```
1.Modify the `/views/js/main.js` to eliminate Forced Synchronous Layout for scroll action.

	```// Moves the sliding background pizzas based on scroll position
	function updatePositions() {
	  frame++;
	  window.performance.mark("mark_start_frame");

	  var items = document.querySelectorAll('.mover');
	  var scrollTop=(document.body.scrollTop / 1250);
	 
	  for (var i = 0; i < items.length; i++) {
	     var phase = Math.sin( scrollTop+ (i % 5));
	    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
	  }

	  // User Timing API to the rescue again. Seriously, it's worth learning.
	  // Super easy to create custom metrics.
	  window.performance.mark("mark_end_frame");
	  window.performance.measure("measure_frame_duration", "mark_start_frame", "mark_end_frame");
	  if (frame % 10 === 0) {
	    var timesToUpdatePosition = window.performance.getEntriesByName("measure_frame_duration");
	    logAverageFrame(timesToUpdatePosition);
	  }
	}

	// runs updatePositions on scroll
	window.addEventListener('scroll', updatePositions);
	```

### Optimization Tips and Tricks
* [Optimizing Performance](https://developers.google.com/web/fundamentals/performance/ "web performance")
* [Analyzing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "analyzing crp")
* [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "optimize the crp!")
* [Avoiding Rendering Blocking CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "render blocking css")
* [Optimizing JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [Measuring with Navigation Timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api"). We didn't cover the Navigation Timing API in the first two lessons but it's an incredibly useful tool for automated page profiling. I highly recommend reading.
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">The fewer the downloads, the better</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">Reduce the size of text</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">Optimize images</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP caching</a>

### Customization with Bootstrap
The portfolio was built on Twitter's <a href="http://getbootstrap.com/">Bootstrap</a> framework. All custom styles are in `dist/css/portfolio.css` in the portfolio repo.

* <a href="http://getbootstrap.com/css/">Bootstrap's CSS Classes</a>
* <a href="http://getbootstrap.com/components/">Bootstrap's Components</a>
