Работа со свойствами
===


<div id="article">

	<div id="toc">
	<div class="toc-top"></div>
	
	

<p></p>
<p>Properties are a central aspect of more advanced testing with soapUI (a general overview of property-management is available at ..). In regard to Functional Testing properties are used to parameterize the execution and functionality of your tests, for example:</p>
<ul>
<li>Properties can be used to hold the endpoints of your services, making it easy to change the actual endpoints used during test execution (see example below)</li>
<li>Properties can be used to hold authentication credentials, making it easy to manage these in a central place or external file</li>
<li>Properties can be used to transfer and share session ids during test execution, so multiple teststeps or testcases can share the same sessions</li>
<li>etc</li>
</ul>
<p>Lets start at looking some of the basics before we dive into an example.</p>
<h1 id="1-defining-properties">1.&nbsp;Defining Properties</h1>
<p>Custom can be defined at several levels in soapUI;</p>
<ul>
<li>At the Project, TestSuite and TestCase level in the corresponding Properties tab (see below)</li>
<li>In a Properties TestStep (see below)</li>
<li>In a DataGen TestStep (<a href="http://www.soapui.org/Functional-Testing/datagen-test-step.html">read more</a>)</li>
<li>As part of a TestStep configuration&nbsp;                    
<ul>
<li>Within a DataSource TestStep (<a href="http://www.soapui.org/Data-Driven-Testing/datasources.html">read more</a>) for performing Data-Driven Testing scenarios</li>
<li>Within a DataSink TestStep (<a href="http://www.soapui.org/Data-Driven-Testing/datasinks.html">read more</a>) for saving property values to external storage</li>
</ul>
</li>
</ul>
<p>Furthermore, most other TestSteps expose properties for both reading and writing, for example</p>
<ul>
<li>the Script TestStep exposes a "Result" property containing the string value returned by the last executed script</li>
<li>all Request TestSteps expose a Response property containing the last received response </li>
</ul>
<br>
<p>Properties can easily be both read and written from scripts and also transferred between TestSteps with the Property-Transfer TestStep (<a href="http://www.soapui.org/Functional-Testing/transfering-property-values.html">read more</a>).</p>
<h1><strong>Project, TestSuite and TestCase Properties</strong></h1>
<p>Properties can be defined at any of these three levels in the corresponding properties inspector, for example at the project level:</p>
<p style="text-align: center;"><img width="631" height="464" alt="project-properties" src="/images/stories/functionaltesting/project-properties.png"></p>
<p style="text-align: left;">Here you can see the defined "Some Property" property both in the Properties tab in the Overview tab of the Project window (to the right), and in the Custom Properties tab for the project node in the Navigator to the left. Corresponding tabs are available for TestSuites and TestCases and all can be used to add/remove/change the contained properties.</p>
<p>Defining properties at these levels allows them to easily be accessed from scripts (see below) and via property expansion (see below), for example a global password might be stored at the project level and accessed inside a request message with a standard property expansion; ${#Project#Password}.</p>
<p>An flexible mechanism for overriding any properties defined at these levels from the command-line is described <a href="http://www.soapui.org/Scripting-Properties/working-with-properties.html#2-setting-properties-from-the-command-line">here</a>.</p>
<h1 id="2-the-properties-teststep">2.&nbsp;The Properties TestStep</h1>
<p>The Properties TestStep is used for defining custom properties to be used within a TestCase. Its main advantages over defining properties at the TestCase level are:</p>
<ul>
<li>You can organize properties into multiple Properties TestSteps (if you have many of them)</li>
<li>You can specify source and target filenames which will be used to read and write the contained properties when the TestStep is executed</li>
</ul>
<p>The Properties TestStep Window is as follows:</p>
<p><img width="586" height="196" alt="properties-teststep-window" src="/images/stories/functionaltesting/properties-teststep-window.png"></p>
<p>Here you can see two defined properties which are read from the specified login.txt file on execution of the TestStep.</p>
<h1 id="3-script-access-to-properties">3.&nbsp;Script access to properties</h1>
<p>Reading and Writing property values from a script is straight-forward, to get a property value you first need to get hold of the containing object and then use the getPropertyValue(..) method. For example to get a TestSuite property from within a Script TestStep you would do the following:</p>
<div id="highlighter_535744" class="syntaxhighlighter "><div class="bar"><div class="toolbar"><a href="#viewSource" title="view source" style="width: 16px; height: 16px;" class="item viewSource">view source</a><a href="#printSource" title="print" style="width: 16px; height: 16px;" class="item printSource">print</a><a href="#about" title="?" style="width: 16px; height: 16px;" class="item about">?</a></div></div><div class="lines"><div class="line alt1"><code class="number">1.</code><span class="content"><span style="margin-left: 0px !important;" class="block"><code class="comments">// get username property from TestSuite</code></span></span></div><div class="line alt2"><code class="number">2.</code><span class="content"><span style="margin-left: 0px !important;" class="block"><code class="keyword">def</code> <code class="plain">username = testRunner.testCase.testSuite.getPropertyValue( </code><code class="string">"Username"</code> <code class="plain">)</code></span></span></div></div></div>
<p>The property had been defined in the TestSuite window as follows:</p>
<p style="text-align: center;"><img width="508" height="174" alt="sample-testsuite-property" src="/images/stories/functionaltesting/sample-testsuite-property.png"></p>
<p>And to write this value to (for example) a HTTP Request Parameter property you would do</p>
<div id="highlighter_996186" class="syntaxhighlighter "><div class="bar"><div class="toolbar"><a href="#viewSource" title="view source" style="width: 16px; height: 16px;" class="item viewSource">view source</a><a href="#printSource" title="print" style="width: 16px; height: 16px;" class="item printSource">print</a><a href="#about" title="?" style="width: 16px; height: 16px;" class="item about">?</a></div></div><div class="lines"><div class="line alt1"><code class="number">1.</code><span class="content"><span style="margin-left: 0px !important;" class="block"><code class="comments">// write the username to the HTTP Request</code></span></span></div><div class="line alt2"><code class="number">2.</code><span class="content"><span style="margin-left: 0px !important;" class="block"><code class="plain">testRunner.testCase.testSteps[</code><code class="string">"HTTP Request"</code><code class="plain">].setPropertyValue( </code><code class="string">"Username"</code><code class="plain">, username )</code></span></span></div></div></div>
<p>(please note that this example could have been achieved much more easily with a Property-Transfer TestStep).</p>
<h1 id="4-example-n-centralized-endpoint">4.&nbsp;Example &nbsp;- Centralized Endpoint</h1>
<p>A common scenario in a more complex service environment is the need to change the endpoint of some of the services involved in your tests, for example between test, dev and staging environments. Manually changing endpoints is of course possible but far too tedious when there might be hundreds of request teststeps involved, and using the command-line host override option does not allow you to change a single service endpoint if you have multiple ones in use. Properties to the rescue;</p>
<ol>
<li>Define a project property holding the endpoint:<br><br><br><br></li>
<p style="text-align: center;"><img width="335" height="286" alt="service-endpoint-property" src="/images/stories/functionaltesting/service-endpoint-property.png"></p>
<li>Configure the endpoint to use this property via property-expansion<br><br><br><br></li>
<p style="text-align: center;"><img width="495" height="200" alt="service-endpoints-tab" src="/images/stories/functionaltesting/service-endpoints-tab.png"></p>
<li>Make sure your requests are using the configured endpoint<br><br><br><br></li>
<p style="text-align: center;"><img width="584" height="172" alt="request-service-endpoint" src="/images/stories/functionaltesting/request-service-endpoint.png"></p>
<li>Now when you run the request, the property will automatically be replaced with its current value. To use a different value just change the endpoint in the UI, or from the command-line you can use the -P option;<br><br><span style="font-family: 'courier new', courier;">-PServiceEndoint=dev.eviware.com:8884</span><br><br>which would use the dev.eviware.com:8884 endpoint instead (entirely fictional). </li>
</ol>

<script type="text/javascript">SyntaxHighlighter.config.stripBrs=true;
		SyntaxHighlighter.all();
	  </script></div>