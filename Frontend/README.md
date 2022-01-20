# WebApplication

Questions & temp notes  

## What's the difference between:
```
// Working
$(document).on("click", ".add-project-btn", function() {
    console.log($(this).parents(".project-container").);
});

// Not working
$(".add-project-btn").on("click", function() {
    console.log($(this).parents(".project-container").);
});
```


```
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section {
    display: block;
}
```

```
@media screen and (max-width: 600px) {
    ul.topnav li.right,
    ul.topnav li {float: none;}
}

http://www.w3schools.com/css/tryit.asp?filename=trycss_navbar_horizontal_responsive
```

## Why doesn't work with "#navbar-container"?
```
#navbar-container button {
	border: none;
	background: none;
	display:inline-block;
	margin: 0;
	padding: 5px 10px;
	font-weight: 400;
}
```

2. Cross origin: Cross origin requests are only supported for protocol schemes: run your html on a simple server to get rid of this:    

	howler.min.js:2 XMLHttpRequest cannot load file:///C:/Users/Jiabei/Desktop/Web/partatap/src/sounds/zig-zag.mp3. Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, https, chrome-extension-resource.
	
	I decided to run it on a local HTTP server hosted by Python, using:
	
	```
	python -m SimpleHTTPServer
	```
	
## If there should be no IDs but only classes, what are we going to do with label and input?

## use "$(this)" inside a listener to represent the element to be bounded

```
    var filters = $(".plan-filter-options .font-subtitle");
    for (var i = 0; i < filters.length; i++) {
        $(filters[i]).on("tap", function() {
            $(this).next().slideToggle(200);
            $(this).siblings(".font-subtitle").next().css("display", "none");
        });
    }
```

## Get selected text
```
val() only gives the "value" of the option, like value="name"

user["gender"] = $("#register-gender option:selected").text();

// Get a list of checked:
var categories = $("input[name='interests']:checked");
```
	

## Knowledge Structure of Frontend Development
- Frontend Engineer
    - Web browsers
        - IE6/[7](http://www.microsoft.com/en-us/download/internet-explorer-7-details.aspx)/[8](http://windows.microsoft.com/en-US/internet-explorer/downloads/ie-8)/[9](http://windows.microsoft.com/en-US/internet-explorer/downloads/ie-9/worldwide-languages)/[10](http://windows.microsoft.com/en-US/internet-explorer/ie-10-worldwide-languages)/[11](http://windows.microsoft.com/en-US/internet-explorer/ie-11-worldwide-languages) (Trident)
        - [Firefox](http://www.mozilla.org/en-US/) (Gecko)
        - [Chrome](http://www.google.com/chrome)/[Chromium](http://www.chromium.org/) (Blink)
        - [Safari](http://www.apple.com/safari/) (WebKit)
        - [Opera](http://www.opera.com/) (Blink)
    - Programming Languages
        - [JavaScript](https://developer.mozilla.org/en-US/docs/JavaScript)/[Node.js](http://nodejs.org/)
        - [CoffeeScript](http://coffeescript.org/)
        - [TypeScript](http://www.typescriptlang.org/)
    - Slicing
        - [HTML](http://www.w3.org/html/)/[HTML5](http://www.w3.org/TR/html5/)
        - [CSS](http://www.w3.org/Style/CSS/)/CSS3
        - [Sass](http://sass-lang.com/)/[LESS](http://lesscss.org/)/[Stylus](http://learnboost.github.io/stylus/)/[PostCSS](http://postcss.org/)
        - [PhotoShop](http://www.photoshop.com/products/photoshop)/[Paint.net](http://www.getpaint.net/)/[Fireworks](http://www.adobe.com/cn/products/fireworks.html)/[GIMP](http://www.gimp.org/)/[Sketch](http://bohemiancoding.com/sketch/)
    - Development tools
        - Editors and IDEs
            - [VIM](http://www.vim.org/)/[Sublime Text2](http://www.sublimetext.com/)
            - [Notepad++](http://notepad-plus-plus.org/)/[EditPlus](http://www.editplus.com/)
            - [WebStorm](http://www.jetbrains.com/webstorm/)
            - [Emacs](http://www.gnu.org/software/emacs/)  [EmacsWiki](http://emacswiki.org)
            - [Brackets](http://brackets.io)
            - [Atom](https://atom.io/)
            - [Lime Text](http://limetext.org/)
            - [Light Table](http://lighttable.com/)
            - [Codebox](https://www.codebox.io/)
            - [TextMate](http://macromates.com/)
            - [Neovim](http://neovim.org/)
            - [Komodo IDE / Edit](http://www.activestate.com/komodo-edit)
            - [Eclipse](http://www.eclipse.org/)
            - [Visual Studio](http://www.visualstudio.com/)/[Visual Studio Code](https://code.visualstudio.com/)
            - [NetBeans](https://netbeans.org/)
            - [Cloud9 IDE](http://c9.io/)
            - [HBuilder](http://www.dcloud.io/)
            - [Nuclide](http://nuclide.io/)
        - Debugging
            - [Firebug](http://getfirebug.com/)/[Firecookie](https://addons.mozilla.org/en-US/firefox/addon/firecookie/)
            - [YSlow](http://developer.yahoo.com/yslow/)
            - [IEDeveloperToolbar](http://www.microsoft.com/en-us/download/details.aspx?id=18359)/[IETester](http://www.my-debugbar.com/wiki/IETester/HomePage)
            - [Fiddler](http://www.telerik.com/fiddler)
            - [Chrome Dev Tools](https://developer.chrome.com/devtools)
            - [Dragonfly](http://www.opera.com/dragonfly/)
            - [DebugBar](http://www.debugbar.com/)
            - [Venkman](https://developer.mozilla.org/en-US/docs/Venkman)
            - [Charles](https://www.charlesproxy.com/)
        - Revision Control
            - [Git](http://git-scm.com/)/[SVN](http://subversion.apache.org/)/[Mercurial](http://mercurial.selenic.com/)
            - [Github](https://github.com/)/[GitLab](https://about.gitlab.com/)/[Bitbucket](https://bitbucket.org/)/[Google Code](http://code.google.com/hosting/)/[Gitorious](https://gitorious.org/)/[GNU Savannah](http://savannah.gnu.org/)/[Launchpad](https://launchpad.net/)/[SourceForge](http://sourceforge.net/)/[TeamForge](http://www.collab.net/products/teamforge)
    - Code quality
        - Coding style
            - [JSLint](http://www.jslint.com/)/[JSHint](http://www.jshint.com/)/[jscs](https://github.com/mdevils/node-jscs)
            - [CSSLint](http://csslint.net/)
            - [Markup Validation Service](http://validator.w3.org/)
            - [HTML Validators](https://validator.whatwg.org/)
        - Unit testing
            - [QUnit](http://qunitjs.com/)/[Jasmine](http://jasmine.github.io/)
            - [Mocha](http://mochajs.org/)/[Should](https://github.com/visionmedia/should.js/)/[Chai](http://chaijs.com/)/[Expect](https://github.com/LearnBoost/expect.js/)
            - [Unit JS](http://unitjs.com/)
        - Auto-testing
            - [WebDriver](http://docs.seleniumhq.org/docs/03_webdriver.jsp)/[Protractor](https://github.com/angular/protractor)/[Karma Runner](https://github.com/karma-runner/karma)/[Sahi](http://sahi.co.in/)
            - [phantomjs](http://phantomjs.org/)
            - [SourceLabs](https://saucelabs.com/)/[BrowserStack](http://www.browserstack.com/)
    - Frontend libraries / frameworks
        - [jQuery](http://jquery.com/)/[Underscore](http://underscorejs.org/)/[Mootools](http://mootools.net/)/[Prototype.js](http://www.prototypejs.org/)
        - [YUI3](http://yuilibrary.com/projects/yui3/)/[Dojo](http://dojotoolkit.org/)/[ExtJS](http://www.sencha.com/products/extjs)/[KISSY](http://docs.kissyui.com/)
        - [Backbone](http://backbonejs.org/)/[KnockoutJS](http://knockoutjs.com/)/[Emberjs](http://emberjs.com/)
        - [AngularJS](http://angularjs.org/)
            - [Batarang](https://chrome.google.com/webstore/detail/angularjs-batarang/ighdmehidhipcmcojjgiloacoafjmpfk)
        - [Bootstrap](http://getbootstrap.com/)
        - [Semantic UI](http://www.semantic-ui.com/)
        - [Juice UI](http://juiceui.com/)
        - [Web Atoms](http://webatomsjs.neurospeech.com/)
        - [Polymer](https://www.polymer-project.org/)
        - [Dhtmlx](http://dhtmlx.com/)
        - [qooxdoo](http://qooxdoo.org/)
        - [React](http://facebook.github.io/react/)
        - [Brick](http://mozbrick.github.io/)
    - Frontend standards / specifications
        - HTTP/1.1: RFCs 7230-7235
        - [HTTP/2](https://http2.github.io/)
        - [ECMAScript3/5](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
        - [W3C: DOM/BOM/XHTML/XML/JSON/JSONP/...](http://www.w3.org/TR/)
        - [CommonJS Modules](http://wiki.commonjs.org/wiki/Modules/1.0)/[AMD](https://github.com/amdjs/amdjs-api/wiki/AMD)
        - [HTML5](http://www.w3.org/html/wg/drafts/html/master/)/[CSS3](http://www.w3.org/Style/CSS/specs.en.html)
        - [Semantic Web](http://semanticweb.org/)
            - [MicroData](http://schema.org)
            - [RDFa](http://www.w3.org/TR/rdfa-core/)
        - [Web Accessibility](http://www.w3.org/WAI/)
            - [WCAG](http://www.w3.org/TR/WAI-WEBCONTENT/)
            - [Role Attribute](http://www.w3.org/TR/role-attribute/)
            - [WAI-ARIA](http://www.w3.org/TR/wai-aria/)
    - Performance
        - [JSPerf](http://jsperf.com/)
        - [YSlow 35 rules](http://developer.yahoo.com/performance/rules.html)
        - [PageSpeed](https://developers.google.com/speed/pagespeed/)
        - [HTTPWatch](http://www.httpwatch.com/)
        - [DynaTrace's Ajax](http://www.compuware.com/application-performance-management/dynatrace-ajax-download.html)
        - [High-performance JavaScript](http://book.douban.com/subject/5362856/)
    - SEO
    - General programming knowledge
        - [Data structure](http://en.wikipedia.org/wiki/Data_structure)
        - OOP/AOP
        - [Prototype chain](http://net.tutsplus.com/tutorials/javascript-ajax/prototypes-in-javascript-what-you-need-to-know/)/Scope chain
        - [Closure](http://www.jibbering.com/faq/notes/closures/)
        - [Programming paradigms](http://en.wikipedia.org/wiki/Programming_paradigm)
        - [Design Patterns](http://addyosmani.com/resources/essentialjsdesignpatterns/book/)
        - [Javascript Tips](http://sanshi.me/articles/JavaScript-Garden-CN/html/index.html)
    - Deployment flow
        - Compressing and merging
            - [YUI Compressor](http://developer.yahoo.com/yui/compressor/)
            - [Google Clousure Complier](https://developers.google.com/closure/compiler/)
            - [UglifyJS](https://github.com/mishoo/UglifyJS)
            - [CleanCSS](https://github.com/GoalSmashers/clean-css)
        - Documentation generating
            - [JSDoc](http://code.google.com/p/jsdoc-toolkit/)
            - [Dox](https://github.com/visionmedia/dox)/[Doxmate](https://github.com/JacksonTian/doxmate)/[Grunt-Doxmate](https://github.com/luozhihua/grunt-doxmate)
        - Building
            - [make](http://www.gnu.org/software/make/)/[Ant](http://ant.apache.org/)
            - [GYP](http://code.google.com/p/gyp/)
            - [Grunt](http://gruntjs.com/)
            - [Gulp](http://gulpjs.com/)
            - [Yeoman](http://yeoman.io/)
            - [FIS](http://fis.baidu.com/)
            - [Mod](https://github.com/modulejs/modjs)
        - ES6+ transpiler
            - [Traceur](https://github.com/google/traceur-compiler)
            - [Babel](https://babeljs.io/)
    - Code organizing
        - Modularizing libraries
            - [CommonJS](http://www.commonjs.org/)/AMD
            - YUI3 Modules
        - Modularizing business logic
            - [bower](https://github.com/twitter/bower)/[component](https://github.com/component/component)
        - File loaders
            - [LABjs](http://labjs.com/)
            - [SeaJS](http://seajs.org/)/[Require.js](http://requirejs.org/)
        - Modular preprocessor
            - [Browserify](https://github.com/substack/node-browserify)
    - Security
        - [CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery)/[XSS](http://en.wikipedia.org/wiki/Cross-site_scripting)
        - [CSP](http://www.w3.org/TR/CSP/)
        - [Same-origin policy](https://developer.mozilla.org/docs/Web/Security/Same-origin_policy)
        - ADsafe/Caja/Sandbox
    - Mobile Web
        - HTML5/CSS3
        - [Responsive web design](http://en.wikipedia.org/wiki/Responsive_web_design)
        - [Zeptojs](http://zeptojs.com/)/[iScroll](http://cubiq.org/iscroll)
        - V5/[Sencha Touch](http://www.sencha.com/products/touch)
        - [PhoneGap](http://phonegap.com/)
        - [jQuery Mobile](http://jquerymobile.com/)
        - [W3C Mobile Web Initiative](http://www.w3.org/Mobile/)
        - [W3C mobileOK Checker](http://validator.w3.org/mobile/)
        - [Open Mobile Alliance](http://openmobilealliance.org/)
        - [React Native](https://facebook.github.io/react-native/)
    - Advanced technology communities/conferences
        - [D2](http://d2forum.org)/[WebRebuild](http://www.webrebuild.org/)
        - NodeParty/[W3CTech](http://w3ctech.com)/[HTML5梦工厂(HTML5 Dreamworks)](http://www.html5dw.com)
        - [JSConf](http://jsconf.com/)/[沪JS(JSConf.cn)](http://jsconf.cn)
        - QCon/Velocity/SDCC
        - [JSConf](http://jsconf.com/)/[NodeConf](http://www.nodeconf.com/)
        - [CSSConf](http://cssconf.com/)
        - YDN/YUIConf
        - HybridApp
        - [WHATWG](http://whatwg.org/)
        - [MDN](https://developer.mozilla.org/en-US/)
        - [codepen](http://codepen.io/)
        - [w3cplus](http://www.w3cplus.com/)
        - [CNode](https://cnodejs.org/)
    - Computer science
        - Compilation principle
        - [Computer network](http://en.wikipedia.org/wiki/Computer_network)
        - [OS](http://en.wikipedia.org/wiki/Operating_system)
        - Algorithm principle
        - Software Engineering/Software testing principle
        - [Unicode](http://www.unicode.org/)
    - Soft skills
        - Knowledge Management/Sharing
        - Communication/Teamwork
        - Requirements Management/PM
        - Interaction Design/Availability/Accessibility
    - Visualization
        - SVG/Canvas/VML
        - SVG: [D3](http://d3js.org/)/[Raphaël](http://raphaeljs.com/)/[Snap.svg](http://snapsvg.io/)/[DataV](http://datavlab.org/datavjs/)
        - Canvas: [CreateJS](http://www.createjs.com/)/[KineticJS](http://kineticjs.com/)
        - [WebGL](http://zh.wikipedia.org/wiki/WebGL)/[Three.JS](http://threejs.org/)
- Backend Engineer
    - Programming languages
        - C/C++/Java/PHP/Ruby/Python/...
    - Web server
        - [Nginx](http://nginx.org/en/)
        - [Apache](http://httpd.apache.org/)
        - [Lighttpd](http://www.lighttpd.net/)
    - Databases
        - SQL
        - [MySQL](http://www.mysql.com/)/[PostgreSQL](http://www.postgresql.org/)/[Oracle](http://www.oracle.com/us/products/database/overview/index.html)/[DB2](http://www-01.ibm.com/software/data/db2)
        - [MongoDB](http://www.mongodb.org/)/[CouchDB](http://couchdb.apache.org/)
    - Data Caching
        - [Redis](http://redis.io/)
        - [Memcached](http://memcached.org/)
    - File Cashing/Proxying
        - [Varnish](https://www.varnish-cache.org/)
        - [Squid](http://www.squid-cache.org/)
    - OS
        - Unix/Linux/OS X/Windows
    - Data structure

## Book Recommendations for Frontend-ers
The ★ less and easier, the more suitable for starters

### CSS
- [More Eric Meyer on CSS](http://www.amazon.com/More-Eric-Meyer-Voices-Matter/dp/0735714258/)★★★
- [CSS: The Definitive Guide (3rd Edition)](http://www.amazon.com/CSS-Definitive-Eric-A-Meyer/dp/0596527330/)★★
- [CSS Mastery: Advanced Web Standards Solutions](http://www.amazon.com/CSS-Mastery-Advanced-Standards-Solutions/dp/1430223979/)★★★

### JavaScript
- [DOM Scripting: Web Design with JavaScript and the Document Object Model](http://www.amazon.com/DOM-Scripting-Design-JavaScript-Document/dp/1430233893/)★
- [Professional JavaScript for Web Developsers (Third Edition)](http://www.amazon.com/Professional-JavaScript-Developers-Nicholas-Zakas/dp/1118026691/)★★
- [锋利的jQuery](http://book.douban.com/subject/10792216/)★★
- [High Performance JavaScript](ihttp://www.amazon.com/Performance-JavaScript-Faster-Application-Interfaces/dp/059680279X/)★★★
- [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742/)★★★
- [JavaScript: The Definitive Guide (Sixth Edition)](http://www.amazon.com/JavaScript-Definitive-Guide-Activate-Guides/dp/0596805527/)★★★
- [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680/)★★★
- [JAVASCRIPT语言精髓与编程实践](http://book.douban.com/subject/3012828/)★★★
- [Effective Javascript](http://www.amazon.com/Effective-JavaScript-Specific-Software-Development/dp/0321812182)★★★
- [Secrets of the JavaScript Ninja](http://book.douban.com/subject/3176860/)★★★
- [JavaScript Patterns](http://www.amazon.cn/JavaScript-Patterns-Stefanov-Stoyan/dp/0596806752)★★★
- [JavaScript设计模式](http://book.douban.com/subject/3329540/)★★★★
- [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X)★★★★

### Performance && Practice
- [Web性能实践日志](http://book.douban.com/subject/25891125/)★★★
- [Web性能权威指南](http://book.douban.com/subject/25856314/)★★★

### CVS
- [Pragmatic Version Control Using Git](http://book.douban.com/subject/3105192/)★★
- [Pro Git](http://git-scm.com/book)★★★
- [Git权威指南](http://book.douban.com/subject/6526452/)★★★★

## Front-end Developer Interview
- [Front-end Developer Interview Questions](https://github.com/darcyclarke/Front-end-Developer-Interview-Questions)
- [Front end Developer Questions( Chinese (Simple) Language)](https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions/Question)

References: [JacksonTian](https://github.com/JacksonTian/fks)
