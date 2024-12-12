---


---

<h1 id="callback-hell-explanation-and-solutions">Callback Hell: Explanation and Solutions</h1>
<h2 id="what-is-callback-hell">What is Callback Hell?</h2>
<p>Callback hell occurs when multiple asynchronous operations are nested within each other in a way that makes the code:</p>
<ul>
<li><strong>Hard to read</strong>: Deeply nested structure (commonly called the “pyramid of doom”).</li>
<li><strong>Difficult to debug</strong>: Errors are harder to trace due to nested callbacks.</li>
<li><strong>Challenging to maintain</strong>: Small changes require revisiting and modifying the entire callback chain.</li>
</ul>
<hr>
<h2 id="example-without-callback-hell">Example Without Callback Hell</h2>
<p>Here’s a simple example of reading a single file using <code>fs.readFile</code> in Node.js:</p>
<pre class=" language-javascript"><code class="prism  language-javascript">fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'file.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf-8'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">,</span> data<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token keyword">throw</span> err<span class="token punctuation">;</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>data<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<p>This is straightforward because it involves only one asynchronous operation.</p>
<hr>
<h2 id="example-of-callback-hell">Example of Callback Hell</h2>
<p>When multiple asynchronous operations need to be performed sequentially, the code can quickly become deeply nested:</p>
<pre class=" language-javascript"><code class="prism  language-javascript"> fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'file1.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf-8'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">,</span> data1<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
    <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token keyword">throw</span> err<span class="token punctuation">;</span>
 console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File 1 content:'</span><span class="token punctuation">,</span> data1<span class="token punctuation">)</span><span class="token punctuation">;</span>
  
 fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'file2.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf-8'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">,</span> data2<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
      <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token keyword">throw</span> err<span class="token punctuation">;</span>
  
 console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File 2 content:'</span><span class="token punctuation">,</span> data2<span class="token punctuation">)</span><span class="token punctuation">;</span>
   fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'file3.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf-8'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">,</span> data3<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token keyword">throw</span> err<span class="token punctuation">;</span> console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File 3 content:'</span><span class="token punctuation">,</span> data3<span class="token punctuation">)</span><span class="token punctuation">;</span>      
  	    <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  	 <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h3 id="problems-with-callback-hell">Problems with Callback Hell:</h3>
<ol>
<li><strong>Deep Nesting</strong>: Makes the code harder to read and understand.</li>
<li><strong>Error Propagation</strong>: Each callback needs individual error handling.</li>
<li><strong>Reduced Maintainability</strong>: Adding or modifying functionality is cumbersome.</li>
<li><strong>Debugging Challenges</strong>: Hard to trace the source of errors in deeply nested structures.</li>
</ol>
<hr>
<h2 id="solutions-to-callback-hell">Solutions to Callback Hell</h2>
<h3 id="using-promises">1. Using Promises</h3>
<p>Promises allow chaining of asynchronous operations, reducing nesting and centralizing error handling.</p>
<h4 id="example">Example:</h4>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> fs <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'fs'</span><span class="token punctuation">)</span><span class="token punctuation">.</span>promises<span class="token punctuation">;</span>
  
 fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'file1.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf-8'</span><span class="token punctuation">)</span>
    <span class="token punctuation">.</span><span class="token function">then</span><span class="token punctuation">(</span>data1 <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
      console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File 1 content:'</span><span class="token punctuation">,</span> data1<span class="token punctuation">)</span><span class="token punctuation">;</span>
      <span class="token keyword">return</span> fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'file2.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf-8'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">)</span>
    <span class="token punctuation">.</span><span class="token function">then</span><span class="token punctuation">(</span>data2 <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
      console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File 2 content:'</span><span class="token punctuation">,</span> data2<span class="token punctuation">)</span><span class="token punctuation">;</span>
      <span class="token keyword">return</span> fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'file3.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf-8'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">)</span>
    <span class="token punctuation">.</span><span class="token function">then</span><span class="token punctuation">(</span>data3 <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
      console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File 3 content:'</span><span class="token punctuation">,</span> data3<span class="token punctuation">)</span><span class="token punctuation">;</span>
      <span class="token comment">// Further processing...</span>
    <span class="token punctuation">}</span><span class="token punctuation">)</span>
    <span class="token punctuation">.</span><span class="token keyword">catch</span><span class="token punctuation">(</span>err <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
      console<span class="token punctuation">.</span><span class="token function">error</span><span class="token punctuation">(</span><span class="token string">'Error reading files:'</span><span class="token punctuation">,</span> err<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">)</span>
</code></pre>
<h4 id="benefits">Benefits:</h4>
<ul>
<li>No deep nesting.</li>
<li>Centralized error handling using `.catch()`.</li>
<li>Code is easier to read and manage.</li>
</ul>
<hr>
<h3 id="using-asyncawait">2. Using Async/Await</h3>
<p><code>async/await</code> allows writing asynchronous code in a synchronous style, making it more readable.</p>
<h4 id="example-1">Example:</h4>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> fs <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'fs'</span><span class="token punctuation">)</span><span class="token punctuation">.</span>promises<span class="token punctuation">;</span>
 
<span class="token keyword">async</span> <span class="token keyword">function</span> <span class="token function">readFiles</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
   <span class="token keyword">try</span> <span class="token punctuation">{</span>
     <span class="token keyword">const</span> data1 <span class="token operator">=</span> <span class="token keyword">await</span> fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'file1.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf-8'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
     console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File 1 content:'</span><span class="token punctuation">,</span> data1<span class="token punctuation">)</span><span class="token punctuation">;</span>
 
<span class="token keyword">const</span> data2 <span class="token operator">=</span> <span class="token keyword">await</span> fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'file2.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf-8'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File 2 content:'</span><span class="token punctuation">,</span> data2<span class="token punctuation">)</span><span class="token punctuation">;</span>
 
<span class="token keyword">const</span> data3 <span class="token operator">=</span> <span class="token keyword">await</span> fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'file3.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf-8'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File 3 content:'</span><span class="token punctuation">,</span> data3<span class="token punctuation">)</span><span class="token punctuation">;</span>
 
<span class="token comment">// Further processing...</span>
   <span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">err</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
     console<span class="token punctuation">.</span><span class="token function">error</span><span class="token punctuation">(</span><span class="token string">'Error reading files:'</span><span class="token punctuation">,</span> err<span class="token punctuation">)</span><span class="token punctuation">;</span>
   <span class="token punctuation">}</span>
 <span class="token punctuation">}</span>
 
<span class="token function">readFiles</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h4 id="benefits-1">Benefits:</h4>
<ul>
<li>Completely avoids nesting.</li>
<li>Easy to debug and trace errors using `try-catch`.</li>
<li>Improves readability and maintainability.</li>
</ul>
<hr>
<h2 id="key-takeaways">Key Takeaways</h2>
<ul>
<li>Callback hell arises when multiple nested callbacks are used for sequential asynchronous operations.</li>
<li>It makes the code messy, hard to read, debug, and maintain.</li>
<li>Modern techniques like <strong>Promises</strong> and <strong>Async/Await</strong> help solve this problem by flattening the structure of asynchronous code.</li>
<li>Node.js (since version 10) fully supports Promises and Async/Await, making them the preferred solutions.</li>
</ul>
<hr>
<h2 id="visualizing-callback-hell-the-pyramid-of-doom">Visualizing Callback Hell: The “Pyramid of Doom”</h2>
<h3 id="callback-hell-example-visual-structure">Callback Hell Example (Visual Structure):</h3>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token function">asyncOperation1</span><span class="token punctuation">(</span><span class="token punctuation">(</span>err<span class="token punctuation">,</span> result1<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>   <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token punctuation">{</span>
     <span class="token function">handleError</span><span class="token punctuation">(</span>err<span class="token punctuation">)</span><span class="token punctuation">;</span>   <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
     <span class="token function">asyncOperation2</span><span class="token punctuation">(</span>result1<span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">,</span> result2<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
       <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token punctuation">{</span>
         <span class="token function">handleError</span><span class="token punctuation">(</span>err<span class="token punctuation">)</span><span class="token punctuation">;</span>
       <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
         <span class="token function">asyncOperation3</span><span class="token punctuation">(</span>result2<span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">,</span> result3<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
           <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token punctuation">{</span>
             <span class="token function">handleError</span><span class="token punctuation">(</span>err<span class="token punctuation">)</span><span class="token punctuation">;</span>
           <span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
             console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Final result:'</span><span class="token punctuation">,</span> result3<span class="token punctuation">)</span><span class="token punctuation">;</span>
           <span class="token punctuation">}</span>
         <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
       <span class="token punctuation">}</span>
     <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>   
     <span class="token punctuation">}</span> <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h3 id="promises-example-flattened-structure">Promises Example (Flattened Structure):</h3>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token function">asyncOperation1</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
   <span class="token punctuation">.</span><span class="token function">then</span><span class="token punctuation">(</span>result1 <span class="token operator">=&gt;</span> <span class="token function">asyncOperation2</span><span class="token punctuation">(</span>result1<span class="token punctuation">)</span><span class="token punctuation">)</span>
   <span class="token punctuation">.</span><span class="token function">then</span><span class="token punctuation">(</span>result2 <span class="token operator">=&gt;</span> <span class="token function">asyncOperation3</span><span class="token punctuation">(</span>result2<span class="token punctuation">)</span><span class="token punctuation">)</span>
   <span class="token punctuation">.</span><span class="token function">then</span><span class="token punctuation">(</span>result3 <span class="token operator">=&gt;</span> console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Final result:'</span><span class="token punctuation">,</span> result3<span class="token punctuation">)</span><span class="token punctuation">)</span>
   <span class="token punctuation">.</span><span class="token keyword">catch</span><span class="token punctuation">(</span>err <span class="token operator">=&gt;</span> <span class="token function">handleError</span><span class="token punctuation">(</span>err<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<hr>
<h1 id="event-loop-in-node.js">Event Loop in Node.js</h1>
<h2 id="what-is-the-event-loop">What is the Event Loop?</h2>
<p>The <strong>Event Loop</strong> is a core concept in Node.js that enables <strong>non-blocking, asynchronous programming</strong>. It is a mechanism that continuously checks for tasks to execute, handling asynchronous operations like I/O, timers, and callbacks efficiently.</p>
<hr>
<h2 id="key-features-of-the-event-loop">Key Features of the Event Loop</h2>
<ol>
<li><strong>Single-threaded</strong>: Node.js uses a single thread to handle multiple concurrent operations.</li>
<li><strong>Non-blocking I/O</strong>: It allows multiple I/O operations without waiting for one to finish before starting another.</li>
<li><strong>Concurrency</strong>: Achieved via an asynchronous model powered by the event loop.</li>
</ol>
<hr>
<h2 id="how-does-the-event-loop-work">How Does the Event Loop Work?</h2>
<p>The event loop consists of several <strong>phases</strong> that execute callbacks in a specific order. Each phase has a <strong>callback queue</strong>, and the loop continues running until all tasks are completed.</p>
<hr>
<h2 id="event-loop-phases">Event Loop Phases</h2>
<p>Here are the primary phases of the event loop, in the order they execute:</p>
<h3 id="timers-phase">1. <strong>Timers Phase</strong></h3>
<ul>
<li>Executes callbacks for <strong>setTimeout</strong> and <strong>setInterval</strong>.</li>
<li>Only callbacks whose timer has expired are executed.</li>
</ul>
<h3 id="pending-callbacks-phase">2. <strong>Pending Callbacks Phase</strong></h3>
<ul>
<li>Executes I/O-related callbacks deferred by the operating system.</li>
<li>For example, errors from TCP or UDP operations.</li>
</ul>
<h3 id="idle-prepare-phase">3. <strong>Idle, Prepare Phase</strong></h3>
<ul>
<li>Used internally by Node.js.</li>
<li>Developers rarely interact with this phase.</li>
</ul>
<h3 id="poll-phase">4. <strong>Poll Phase</strong></h3>
<ul>
<li>Retrieves new I/O events.</li>
<li>Executes I/O-related callbacks such as reading files or network requests.</li>
<li>If the poll queue is empty:
<ul>
<li>If timers are waiting, it proceeds to the next phase.</li>
<li>Otherwise, it waits for new I/O tasks to arrive.</li>
</ul>
</li>
</ul>
<h3 id="check-phase">5. <strong>Check Phase</strong></h3>
<ul>
<li>Executes callbacks for <strong>setImmediate</strong>.</li>
<li>Callbacks from <code>setImmediate</code> are prioritized over <code>setTimeout</code>.</li>
</ul>
<h3 id="close-callbacks-phase">6. <strong>Close Callbacks Phase</strong></h3>
<ul>
<li>Executes callbacks for closed events like <code>socket.on('close')</code>.</li>
</ul>
<hr>
<h2 id="order-of-execution">Order of Execution</h2>
<p>The event loop processes tasks in the following order:</p>
<ol>
<li><strong>Timers</strong> (setTimeout, setInterval)</li>
<li><strong>Pending Callbacks</strong></li>
<li><strong>Idle, Prepare</strong></li>
<li><strong>Poll</strong> (I/O operations)</li>
<li><strong>Check</strong> (setImmediate)</li>
<li><strong>Close Callbacks</strong></li>
</ol>
<hr>
<h2 id="visualizing-the-event-loop">Visualizing the Event Loop</h2>
<p>Here’s a simplified diagram of the event loop phases:</p>
<pre><code>┌───────────────────────────┐
│         Timers            │
│   (setTimeout,setInterval)│
└─────────────┬─────────────┘
              │
┌─────────────▼─────────────┐
│    Pending Callbacks      │
└─────────────┬─────────────┘
              │
┌─────────────▼─────────────┐
│       Poll Phase           │
│(I/O,network requests,etc.) │
└─────────────┬─────────────┘
              │
┌─────────────▼─────────────┐
│      Check Phase          │
│     (setImmediate)        │
└─────────────┬─────────────┘
              │
┌─────────────▼─────────────┐
│    Close Callbacks        │
└───────────────────────────┘
</code></pre>
<hr>
<h2 id="code-example-event-loop-in-action">Code Example: Event Loop in Action</h2>
<p>Here’s an example demonstrating how different tasks are handled in the event loop:</p>
<pre class=" language-javascript"><code class="prism  language-javascript">console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Start'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token function">setTimeout</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Timeout 1'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token function">setImmediate</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Immediate'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

Promise<span class="token punctuation">.</span><span class="token function">resolve</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">then</span><span class="token punctuation">(</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Promise'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'End'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h3 id="output-explanation">Output Explanation:</h3>
<ol>
<li><strong>Synchronous code</strong> (<code>console.log('Start')</code> and <code>console.log('End')</code>) runs first.</li>
<li><strong>Promises</strong> (<code>Promise.resolve()</code>) are part of the microtask queue and run after the current phase completes.</li>
<li><strong>Timers</strong> (<code>setTimeout</code>) execute in the <strong>Timers Phase</strong>.</li>
<li><strong>setImmediate</strong> runs in the <strong>Check Phase</strong>, which follows the Poll Phase.</li>
</ol>
<p>Output:</p>
<pre><code>Start
End
Promise
Timeout 1
Immediate
</code></pre>
<hr>
<h2 id="key-takeaways-1">Key Takeaways</h2>
<ul>
<li>The event loop processes tasks in phases, ensuring efficient handling of asynchronous operations.</li>
<li><strong>Timers (setTimeout/setInterval)</strong> and <strong>setImmediate</strong> are handled in different phases.</li>
<li>Promises are part of the <strong>microtask queue</strong> and are executed before moving to the next phase.</li>
<li>Node.js’s event loop is what makes it highly performant for I/O-intensive applications.</li>
</ul>
<hr>
<h1 id="streams-in-node.js">Streams in Node.js</h1>
<h2 id="what-are-streams">What are Streams?</h2>
<p>Streams in Node.js are <strong>objects</strong> used to handle <strong>continuous data flow</strong>. They are particularly useful for reading or writing large amounts of data in chunks, instead of loading the entire data into memory at once.</p>
<hr>
<h2 id="key-features-of-streams">Key Features of Streams</h2>
<ol>
<li><strong>Efficient Memory Usage</strong>: Streams process data in chunks, avoiding high memory consumption.</li>
<li><strong>Time-Efficient</strong>: Data is processed as it arrives, reducing latency.</li>
<li><strong>Event-Driven</strong>: Streams are built on Node.js’s event-driven architecture.</li>
</ol>
<hr>
<h2 id="types-of-streams">Types of Streams</h2>
<p>Node.js provides <strong>four types of streams</strong>:</p>
<h3 id="readable-streams">1. <strong>Readable Streams</strong></h3>
<ul>
<li>Used to <strong>read data</strong> from a source.</li>
<li>Examples: <code>fs.createReadStream</code>, HTTP <code>request</code> objects.</li>
</ul>
<h4 id="common-methods--events">Common Methods &amp; Events</h4>
<ul>
<li><strong><code>readable.read()</code></strong>: Reads data from the stream.</li>
<li><strong>Events</strong>:
<ul>
<li><code>data</code>: Triggered when a chunk of data is available.</li>
<li><code>end</code>: Triggered when no more data is available.</li>
</ul>
</li>
</ul>
<h4 id="example-2">Example</h4>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> fs <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'fs'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> readable <span class="token operator">=</span> fs<span class="token punctuation">.</span><span class="token function">createReadStream</span><span class="token punctuation">(</span><span class="token string">'file.txt'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

readable<span class="token punctuation">.</span><span class="token function">on</span><span class="token punctuation">(</span><span class="token string">'data'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>chunk<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Received chunk:'</span><span class="token punctuation">,</span> chunk<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

readable<span class="token punctuation">.</span><span class="token function">on</span><span class="token punctuation">(</span><span class="token string">'end'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'No more data.'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<hr>
<h3 id="writable-streams">2. <strong>Writable Streams</strong></h3>
<ul>
<li>Used to <strong>write data</strong> to a destination.</li>
<li>Examples: <code>fs.createWriteStream</code>, HTTP <code>response</code> objects.</li>
</ul>
<h4 id="common-methods--events-1">Common Methods &amp; Events</h4>
<ul>
<li><strong><code>writable.write(chunk)</code></strong>: Writes data to the stream.</li>
<li><strong><code>writable.end()</code></strong>: Signals the end of writing.</li>
<li><strong>Events</strong>:
<ul>
<li><code>drain</code>: Triggered when the stream is ready for more data.</li>
<li><code>finish</code>: Triggered when all data has been written.</li>
</ul>
</li>
</ul>
<h4 id="example-3">Example</h4>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> fs <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'fs'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> writable <span class="token operator">=</span> fs<span class="token punctuation">.</span><span class="token function">createWriteStream</span><span class="token punctuation">(</span><span class="token string">'output.txt'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

writable<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span><span class="token string">'Hello, '</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
writable<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span><span class="token string">'world!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
writable<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
writable<span class="token punctuation">.</span><span class="token function">on</span><span class="token punctuation">(</span><span class="token string">'finish'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'All data written.'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<hr>
<h3 id="duplex-streams">3. <strong>Duplex Streams</strong></h3>
<ul>
<li><strong>Both readable and writable</strong>.</li>
<li>Examples: TCP sockets, <code>net.Socket</code>.</li>
</ul>
<h4 id="example-4">Example</h4>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> <span class="token punctuation">{</span> Duplex <span class="token punctuation">}</span> <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'stream'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">const</span> duplex <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Duplex</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
  <span class="token function">read</span><span class="token punctuation">(</span>size<span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">push</span><span class="token punctuation">(</span><span class="token string">'Hello from Duplex!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">push</span><span class="token punctuation">(</span><span class="token keyword">null</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// End the readable side</span>
  <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token function">write</span><span class="token punctuation">(</span>chunk<span class="token punctuation">,</span> encoding<span class="token punctuation">,</span> callback<span class="token punctuation">)</span> <span class="token punctuation">{</span>
    console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token template-string"><span class="token string">`Writable side received: </span><span class="token interpolation"><span class="token interpolation-punctuation punctuation">${</span>chunk<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token interpolation-punctuation punctuation">}</span></span><span class="token string">`</span></span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token function">callback</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

duplex<span class="token punctuation">.</span><span class="token function">on</span><span class="token punctuation">(</span><span class="token string">'data'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>chunk<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>chunk<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
duplex<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span><span class="token string">'Hello Duplex Stream!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
duplex<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<hr>
<h3 id="transform-streams">4. <strong>Transform Streams</strong></h3>
<ul>
<li>A special type of duplex stream where the <strong>output is a transformation of the input</strong>.</li>
<li>Example: Compression (<code>zlib.createGzip</code>), encryption.</li>
</ul>
<h4 id="example-5">Example</h4>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> <span class="token punctuation">{</span> Transform <span class="token punctuation">}</span> <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'stream'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">const</span> transform <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Transform</span><span class="token punctuation">(</span><span class="token punctuation">{</span>
  <span class="token function">transform</span><span class="token punctuation">(</span>chunk<span class="token punctuation">,</span> encoding<span class="token punctuation">,</span> callback<span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token keyword">this</span><span class="token punctuation">.</span><span class="token function">push</span><span class="token punctuation">(</span>chunk<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">toUpperCase</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token function">callback</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span><span class="token punctuation">,</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

process<span class="token punctuation">.</span>stdin<span class="token punctuation">.</span><span class="token function">pipe</span><span class="token punctuation">(</span>transform<span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">pipe</span><span class="token punctuation">(</span>process<span class="token punctuation">.</span>stdout<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<hr>
<h2 id="stream-modes">Stream Modes</h2>
<p>Streams can operate in two modes:</p>
<ol>
<li><strong>Flowing Mode</strong>:
<ul>
<li>Data is read as soon as it is available.</li>
<li>Listeners (<code>.on('data')</code>) are used to handle chunks.</li>
</ul>
</li>
<li><strong>Paused Mode</strong>:
<ul>
<li>Data must be explicitly read using <code>.read()</code>.</li>
</ul>
</li>
</ol>
<hr>
<h2 id="backpressure-in-streams">Backpressure in Streams</h2>
<p><strong>Backpressure</strong> occurs when the writable stream cannot process incoming data as quickly as it is being supplied by the readable stream. Node.js handles this automatically by pausing and resuming the flow of data.</p>
<hr>
<h2 id="use-cases-of-streams">Use Cases of Streams</h2>
<ol>
<li><strong>File Handling</strong>: Reading/writing large files efficiently.</li>
<li><strong>Networking</strong>: Handling requests and responses in HTTP, TCP.</li>
<li><strong>Data Processing</strong>: Compressing, encrypting, or transforming data.</li>
</ol>
<hr>
<h2 id="key-takeaways-2">Key Takeaways</h2>
<ul>
<li>Streams process data in chunks, ensuring efficiency.</li>
<li>Four types of streams: Readable, Writable, Duplex, Transform.</li>
<li>Streams operate in flowing or paused modes.</li>
<li>Node.js streams are event-driven, leveraging the power of asynchronous programming.</li>
</ul>
<hr>
<h1 id="buffer-and-file-system-in-node.js">Buffer and File System in Node.js</h1>
<h2 id="buffer-in-node.js">Buffer in Node.js</h2>
<p>Buffers in Node.js are <strong>used to handle binary data</strong>. They act as temporary storage units for raw binary data, making them essential for tasks like file I/O or working with streams.</p>
<hr>
<h3 id="key-features-of-buffers">Key Features of Buffers</h3>
<ol>
<li><strong>Raw Binary Data</strong>: Buffers store raw binary data, similar to arrays, but without resizable capabilities.</li>
<li><strong>Global Object</strong>: The <code>Buffer</code> class is a part of the global scope, so it doesn’t require importing.</li>
<li><strong>Encoding Support</strong>: Buffers support different encodings like <code>utf8</code>, <code>base64</code>, <code>hex</code>, etc.</li>
</ol>
<hr>
<h3 id="creating-buffers">Creating Buffers</h3>
<ol>
<li><strong>Allocating a Buffer</strong></li>
</ol>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> buffer <span class="token operator">=</span> Buffer<span class="token punctuation">.</span><span class="token function">alloc</span><span class="token punctuation">(</span><span class="token number">10</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Creates a buffer of 10 bytes</span>
console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>buffer<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<ol start="2">
<li><strong>Buffer from String</strong></li>
</ol>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> buffer <span class="token operator">=</span> Buffer<span class="token punctuation">.</span><span class="token keyword">from</span><span class="token punctuation">(</span><span class="token string">'Hello, Node.js!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>buffer<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Converts back to string</span>
</code></pre>
<ol start="3">
<li><strong>Buffer from Array</strong></li>
</ol>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> buffer <span class="token operator">=</span> Buffer<span class="token punctuation">.</span><span class="token keyword">from</span><span class="token punctuation">(</span><span class="token punctuation">[</span><span class="token number">72</span><span class="token punctuation">,</span> <span class="token number">101</span><span class="token punctuation">,</span> <span class="token number">108</span><span class="token punctuation">,</span> <span class="token number">108</span><span class="token punctuation">,</span> <span class="token number">111</span><span class="token punctuation">]</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>buffer<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Output: Hello</span>
</code></pre>
<h3 id="common-buffer-methods">Common Buffer Methods</h3>
<ul>
<li><strong><code>toString()</code></strong>: Converts buffer to string.</li>
<li><strong><code>slice(start, end)</code></strong>: Returns a portion of the buffer.</li>
<li><strong><code>write(string)</code></strong>: Writes a string into the buffer.</li>
<li><strong><code>length</code></strong>: Returns the buffer length.</li>
</ul>
<hr>
<p><strong>Example</strong></p>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> buffer <span class="token operator">=</span> Buffer<span class="token punctuation">.</span><span class="token keyword">from</span><span class="token punctuation">(</span><span class="token string">'Node.js'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>buffer<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token string">'utf8'</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Output: Node.js</span>
console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>buffer<span class="token punctuation">.</span><span class="token function">slice</span><span class="token punctuation">(</span><span class="token number">0</span><span class="token punctuation">,</span> <span class="token number">4</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Output: Node</span>
console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>buffer<span class="token punctuation">.</span>length<span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Output: 6</span>
</code></pre>
<h2 id="file-system-fs-module-in-node.js">File System (fs) Module in Node.js</h2>
<p>The <strong>File System (fs)</strong> module allows you to interact with the file system, enabling operations like reading, writing, appending, and deleting files.</p>
<hr>
<h3 id="key-features-of-the-fs-module">Key Features of the <code>fs</code> Module</h3>
<ol>
<li><strong>Asynchronous and Synchronous APIs</strong>: Methods have both async (non-blocking) and sync (blocking) versions.</li>
<li><strong>Stream Support</strong>: Works seamlessly with streams for efficient file handling.</li>
<li><strong>Error Handling</strong>: Handles system-level errors during file operations.</li>
</ol>
<hr>
<h3 id="common-file-system-methods">Common File System Methods</h3>
<h4 id="reading-files">1. <strong>Reading Files</strong></h4>
<ul>
<li><strong>Asynchronous</strong>:</li>
</ul>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> fs <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'fs'</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 
fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'example.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf8'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">,</span> data<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
	<span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token keyword">throw</span> err<span class="token punctuation">;</span> console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>data<span class="token punctuation">)</span><span class="token punctuation">;</span>
 <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<ul>
<li><strong>Synchronous</strong>:</li>
</ul>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> data <span class="token operator">=</span> fs<span class="token punctuation">.</span><span class="token function">readFileSync</span><span class="token punctuation">(</span><span class="token string">'example.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf8'</span><span class="token punctuation">)</span><span class="token punctuation">;</span> console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>data<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h4 id="writing-files">2. <strong>Writing Files</strong></h4>
<ul>
<li><strong>Asynchronous</strong>:</li>
</ul>
<pre class=" language-javascript"><code class="prism  language-javascript">fs<span class="token punctuation">.</span><span class="token function">writeFile</span><span class="token punctuation">(</span><span class="token string">'output.txt'</span><span class="token punctuation">,</span> <span class="token string">'Hello, World!'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
 <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token keyword">throw</span> err<span class="token punctuation">;</span> console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File written successfully!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
 <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<ul>
<li><strong>Synchronous:</strong></li>
</ul>
<pre class=" language-javascript"><code class="prism  language-javascript">fs<span class="token punctuation">.</span><span class="token function">writeFileSync</span><span class="token punctuation">(</span><span class="token string">'output.txt'</span><span class="token punctuation">,</span> <span class="token string">'Hello, World!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h4 id="appending-data">3. <strong>Appending Data</strong></h4>
<pre class=" language-javascript"><code class="prism  language-javascript">fs<span class="token punctuation">.</span><span class="token function">appendFile</span><span class="token punctuation">(</span><span class="token string">'output.txt'</span><span class="token punctuation">,</span> <span class="token string">'\\nAppended Text'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token keyword">throw</span> err<span class="token punctuation">;</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Data appended!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span>
</code></pre>
<pre class=" language-javascript"><code class="prism  language-javascript">fs<span class="token punctuation">.</span><span class="token function">unlink</span><span class="token punctuation">(</span><span class="token string">'output.txt'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token keyword">throw</span> err<span class="token punctuation">;</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File deleted!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<hr>
<h2 id="streams-with-file-system">Streams with File System</h2>
<p>The <code>fs</code> module integrates well with streams for handling large files efficiently.</p>
<h4 id="example-reading-a-large-file-with-streams">Example: Reading a Large File with Streams</h4>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> readable <span class="token operator">=</span> fs<span class="token punctuation">.</span><span class="token function">createReadStream</span><span class="token punctuation">(</span><span class="token string">'largeFile.txt'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

readable<span class="token punctuation">.</span><span class="token function">on</span><span class="token punctuation">(</span><span class="token string">'data'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>chunk<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Received chunk:'</span><span class="token punctuation">,</span> chunk<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

readable<span class="token punctuation">.</span><span class="token function">on</span><span class="token punctuation">(</span><span class="token string">'end'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Finished reading file.'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h4 id="example-writing-with-streams">Example: Writing with Streams</h4>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> writable <span class="token operator">=</span> fs<span class="token punctuation">.</span><span class="token function">createWriteStream</span><span class="token punctuation">(</span><span class="token string">'output.txt'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

writable<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span><span class="token string">'Writing data in chunks.'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
writable<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token string">'Finished writing!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h2 id="use-cases">Use Cases</h2>
<ul>
<li><strong>Buffer</strong>: Useful for handling binary data like images, videos, or file chunks.</li>
<li><strong>File System</strong>: Provides comprehensive file operations for logging, data storage, or configuration files.</li>
</ul>
<hr>
<h2 id="key-takeaways-3">Key Takeaways</h2>
<ul>
<li><strong>Buffer</strong> is used for raw binary data, while <strong>fs</strong> handles file I/O operations.</li>
<li>The <code>fs</code> module provides both synchronous and asynchronous methods.</li>
<li>Combining Buffers with the <code>fs</code> module ensures efficient handling of files and binary data.</li>
</ul>
<hr>
<h1 id="error-handling-in-node.js">Error Handling in Node.js</h1>
<p>Error handling is a critical part of Node.js applications to ensure that unexpected scenarios don’t crash the application and are handled gracefully.</p>
<hr>
<h2 id="common-error-handling-techniques">Common Error Handling Techniques</h2>
<ol>
<li><strong>Try-Catch Blocks</strong></li>
<li><strong>Promise Chaining</strong></li>
<li><strong>Async/Await with Try-Catch</strong></li>
<li><strong>Error Events with Event Emitters</strong></li>
</ol>
<hr>
<h3 id="using-try-catch-blocks">1. Using Try-Catch Blocks</h3>
<p>This is a basic and widely used mechanism for handling errors in synchronous code.</p>
<h4 id="example-6">Example:</h4>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">try</span> <span class="token punctuation">{</span>
  <span class="token keyword">const</span> data <span class="token operator">=</span> JSON<span class="token punctuation">.</span><span class="token function">parse</span><span class="token punctuation">(</span><span class="token string">'Invalid JSON String'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">error</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">error</span><span class="token punctuation">(</span><span class="token string">'Error parsing JSON:'</span><span class="token punctuation">,</span> error<span class="token punctuation">.</span>message<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<h4 id="key-points">Key Points:</h4>
<ul>
<li>Wrap the risky code in a <code>try</code> block.</li>
<li>Catch and handle the error in the <code>catch</code> block.</li>
</ul>
<hr>
<h3 id="handling-errors-in-promises">2. Handling Errors in Promises</h3>
<p>Promises have built-in mechanisms for error handling using <code>.catch()</code>.</p>
<h4 id="example-7">Example:</h4>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> <span class="token function-variable function">fetchData</span> <span class="token operator">=</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  <span class="token keyword">return</span> <span class="token keyword">new</span> <span class="token class-name">Promise</span><span class="token punctuation">(</span><span class="token punctuation">(</span>resolve<span class="token punctuation">,</span> reject<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
    <span class="token function">reject</span><span class="token punctuation">(</span><span class="token keyword">new</span> <span class="token class-name">Error</span><span class="token punctuation">(</span><span class="token string">'Data fetch failed'</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token function">fetchData</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
  <span class="token punctuation">.</span><span class="token function">then</span><span class="token punctuation">(</span><span class="token punctuation">(</span>data<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>data<span class="token punctuation">)</span><span class="token punctuation">)</span>
  <span class="token punctuation">.</span><span class="token keyword">catch</span><span class="token punctuation">(</span><span class="token punctuation">(</span>error<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> console<span class="token punctuation">.</span><span class="token function">error</span><span class="token punctuation">(</span><span class="token string">'Error:'</span><span class="token punctuation">,</span> error<span class="token punctuation">.</span>message<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h4 id="key-points-1">Key Points:</h4>
<ul>
<li>Errors are propagated to the <code>.catch()</code> block.</li>
<li>Any error in the promise chain is caught.</li>
</ul>
<hr>
<h3 id="using-asyncawait-with-try-catch">3. Using Async/Await with Try-Catch</h3>
<p>When working with <code>async/await</code>, always use a <code>try-catch</code> block to handle errors.</p>
<h4 id="example-8">Example:</h4>
<pre class=" language-javascript"><code class="prism  language-javascript">
<span class="token keyword">const</span> fetchData <span class="token operator">=</span> <span class="token keyword">async</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  <span class="token keyword">try</span> <span class="token punctuation">{</span>
    <span class="token keyword">const</span> data <span class="token operator">=</span> <span class="token keyword">await</span> <span class="token function">fetch</span><span class="token punctuation">(</span><span class="token string">'https://example.com/api'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">const</span> json <span class="token operator">=</span> <span class="token keyword">await</span> data<span class="token punctuation">.</span><span class="token function">json</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span>json<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">error</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    console<span class="token punctuation">.</span><span class="token function">error</span><span class="token punctuation">(</span><span class="token string">'Async/Await Error:'</span><span class="token punctuation">,</span> error<span class="token punctuation">.</span>message<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span>

<span class="token function">fetchData</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h4 id="key-points-2">Key Points:</h4>
<ul>
<li>Encapsulate asynchronous code in a <code>try-catch</code> block.</li>
<li>Prevent unhandled rejections in promises.</li>
</ul>
<hr>
<h3 id="handling-errors-with-event-emitters">4. Handling Errors with Event Emitters</h3>
<p>Node.js uses event-driven programming, and errors can also be handled via event emitters.</p>
<h4 id="example-9">Example:</h4>
<pre class=" language-javascript"><code class="prism  language-javascript">
<span class="token keyword">const</span> EventEmitter <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'events'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">class</span> <span class="token class-name">MyEmitter</span> <span class="token keyword">extends</span> <span class="token class-name">EventEmitter</span> <span class="token punctuation">{</span><span class="token punctuation">}</span>

<span class="token keyword">const</span> myEmitter <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">MyEmitter</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">// Register an error listener</span>
myEmitter<span class="token punctuation">.</span><span class="token function">on</span><span class="token punctuation">(</span><span class="token string">'error'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>error<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">error</span><span class="token punctuation">(</span><span class="token string">'Error caught:'</span><span class="token punctuation">,</span> error<span class="token punctuation">.</span>message<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">// Emit an error</span>
myEmitter<span class="token punctuation">.</span><span class="token function">emit</span><span class="token punctuation">(</span><span class="token string">'error'</span><span class="token punctuation">,</span> <span class="token keyword">new</span> <span class="token class-name">Error</span><span class="token punctuation">(</span><span class="token string">'Something went wrong!'</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h4 id="key-points-3">Key Points:</h4>
<ul>
<li>Use <code>.on('error', callback)</code> to catch emitted errors.</li>
<li>Prevents the application from crashing due to uncaught errors.</li>
</ul>
<hr>
<h2 id="best-practices-for-error-handling">Best Practices for Error Handling</h2>
<ol>
<li><strong>Centralized Error Handling</strong>: Use middleware or utility functions to handle errors centrally.</li>
<li><strong>Graceful Shutdown</strong>: Clean up resources when errors occur.</li>
<li><strong>Avoid Silent Failures</strong>: Log errors for debugging purposes.</li>
<li><strong>Error Codes and Messages</strong>: Provide clear and descriptive error messages.</li>
</ol>
<h4 id="example-of-centralized-error-handling-in-express">Example of Centralized Error Handling in Express:</h4>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> express <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'express'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> app <span class="token operator">=</span> <span class="token function">express</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

app<span class="token punctuation">.</span><span class="token function">use</span><span class="token punctuation">(</span><span class="token punctuation">(</span>req<span class="token punctuation">,</span> res<span class="token punctuation">,</span> next<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  <span class="token function">next</span><span class="token punctuation">(</span><span class="token keyword">new</span> <span class="token class-name">Error</span><span class="token punctuation">(</span><span class="token string">'Page not found'</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">// Forward errors to the error handler</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">// Error handling middleware</span>
app<span class="token punctuation">.</span><span class="token function">use</span><span class="token punctuation">(</span><span class="token punctuation">(</span>err<span class="token punctuation">,</span> req<span class="token punctuation">,</span> res<span class="token punctuation">,</span> next<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">error</span><span class="token punctuation">(</span>err<span class="token punctuation">.</span>message<span class="token punctuation">)</span><span class="token punctuation">;</span>
  res<span class="token punctuation">.</span><span class="token function">status</span><span class="token punctuation">(</span><span class="token number">500</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">send</span><span class="token punctuation">(</span><span class="token string">'Something broke!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

app<span class="token punctuation">.</span><span class="token function">listen</span><span class="token punctuation">(</span><span class="token number">3000</span><span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Server is running on port 3000'</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<hr>
<h2 id="key-takeaways-4">Key Takeaways</h2>
<ul>
<li>Use <code>try-catch</code> for synchronous and <code>async/await</code> for asynchronous code.</li>
<li>Always handle errors in promises using <code>.catch()</code>.</li>
<li>Leverage <code>EventEmitter</code> for handling custom error events.</li>
<li>Centralized error handling is essential in larger applications.</li>
</ul>
<hr>
<h1 id="async-behavior-and-handling-multiple-requests">Async Behavior, and Handling Multiple Requests</h1>
<h2 id="callback-functions-in-node.js">1. Callback Functions in Node.js</h2>
<p>A <strong>callback function</strong> in Node.js is a function passed as an argument to another function, executed after the completion of the asynchronous operation.</p>
<h3 id="example-of-callback-function">Example of Callback Function</h3>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> fs <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'fs'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'example.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf8'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>err<span class="token punctuation">,</span> data<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  <span class="token keyword">if</span> <span class="token punctuation">(</span>err<span class="token punctuation">)</span> <span class="token punctuation">{</span>
    console<span class="token punctuation">.</span><span class="token function">error</span><span class="token punctuation">(</span><span class="token string">'Error reading file:'</span><span class="token punctuation">,</span> err<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">return</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File content:'</span><span class="token punctuation">,</span> data<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<p>In this example:</p>
<ul>
<li><code>fs.readFile</code> is asynchronous.</li>
<li>It doesn’t block the main thread and executes the callback once the file is read.</li>
</ul>
<hr>
<h2 id="io-operations-in-node.js">2. I/O Operations in Node.js</h2>
<p><strong>I/O Operations</strong> (Input/Output operations) involve tasks that require interacting with external systems like:</p>
<ul>
<li>Reading/writing files.</li>
<li>Querying a database.</li>
<li>Making HTTP requests.</li>
</ul>
<h3 id="how-node.js-handles-io">How Node.js Handles I/O</h3>
<p>Node.js delegates I/O operations to:</p>
<ul>
<li><strong>libuv thread pool</strong> (for file system operations).</li>
<li>The operating system (for network requests).</li>
</ul>
<hr>
<h2 id="managing-async-behavior">3. Managing Async Behavior</h2>
<p>When a function depends on an external service or data (like reading a file), async behavior can be managed using:</p>
<ol>
<li><strong>Callbacks</strong></li>
<li><strong>Promises</strong></li>
<li><strong>Async/Await</strong></li>
</ol>
<h3 id="example-with-asyncawait">Example with Async/Await</h3>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> fs <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'fs'</span><span class="token punctuation">)</span><span class="token punctuation">.</span>promises<span class="token punctuation">;</span>

<span class="token keyword">async</span> <span class="token keyword">function</span> <span class="token function">processFile</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">try</span> <span class="token punctuation">{</span>
    <span class="token keyword">const</span> data <span class="token operator">=</span> <span class="token keyword">await</span> fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'example.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf8'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'File content:'</span><span class="token punctuation">,</span> data<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">err</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    console<span class="token punctuation">.</span><span class="token function">error</span><span class="token punctuation">(</span><span class="token string">'Error:'</span><span class="token punctuation">,</span> err<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>

<span class="token function">processFile</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h3 id="why-asyncawait-is-non-blocking">Why Async/Await is Non-Blocking</h3>
<p>While <code>await</code> pauses the current function’s execution, the event loop remains free to handle other requests.</p>
<hr>
<h2 id="handling-multiple-requests-in-an-express-server">4. Handling Multiple Requests in an Express Server</h2>
<p>Node.js uses a <strong>single-threaded event loop</strong> to handle multiple concurrent requests.</p>
<h3 id="example-of-express-server">Example of Express Server</h3>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> express <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'express'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> fs <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'fs'</span><span class="token punctuation">)</span><span class="token punctuation">.</span>promises<span class="token punctuation">;</span>
<span class="token keyword">const</span> app <span class="token operator">=</span> <span class="token function">express</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

app<span class="token punctuation">.</span><span class="token keyword">get</span><span class="token punctuation">(</span><span class="token string">'/data'</span><span class="token punctuation">,</span> <span class="token keyword">async</span> <span class="token punctuation">(</span>req<span class="token punctuation">,</span> res<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  <span class="token keyword">try</span> <span class="token punctuation">{</span>
    <span class="token keyword">const</span> data <span class="token operator">=</span> <span class="token keyword">await</span> fs<span class="token punctuation">.</span><span class="token function">readFile</span><span class="token punctuation">(</span><span class="token string">'example.txt'</span><span class="token punctuation">,</span> <span class="token string">'utf8'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    res<span class="token punctuation">.</span><span class="token function">send</span><span class="token punctuation">(</span><span class="token punctuation">{</span> message<span class="token punctuation">:</span> <span class="token string">'File read successfully'</span><span class="token punctuation">,</span> data <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span> <span class="token keyword">catch</span> <span class="token punctuation">(</span><span class="token class-name">err</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    res<span class="token punctuation">.</span><span class="token function">status</span><span class="token punctuation">(</span><span class="token number">500</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">send</span><span class="token punctuation">(</span><span class="token punctuation">{</span> error<span class="token punctuation">:</span> <span class="token string">'Error reading file'</span> <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

app<span class="token punctuation">.</span><span class="token function">listen</span><span class="token punctuation">(</span><span class="token number">3000</span><span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
  console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'Server running on port 3000'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h3 id="what-happens-with-multiple-requests">What Happens with Multiple Requests</h3>
<ol>
<li><strong>Request Arrival</strong>: Each request is added to the event queue.</li>
<li><strong>Handling I/O</strong>: I/O tasks are delegated to the thread pool or OS, freeing the event loop.</li>
<li><strong>Callback Execution</strong>: When the I/O is complete, the response is sent.</li>
</ol>
<hr>
<h2 id="managing-cpu-bound-tasks">5. Managing CPU-Bound Tasks</h2>
<p>For tasks that require significant computation, Node.js can offload work using:</p>
<ol>
<li><strong>Worker Threads</strong></li>
<li><strong>Clustering</strong></li>
<li><strong>Microservices</strong></li>
</ol>
<h3 id="worker-threads-example">Worker Threads Example</h3>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> <span class="token punctuation">{</span> Worker <span class="token punctuation">}</span> <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'worker_threads'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">new</span> <span class="token class-name">Worker</span><span class="token punctuation">(</span><span class="token string">'./worker.js'</span><span class="token punctuation">,</span> <span class="token punctuation">{</span> workerData<span class="token punctuation">:</span> <span class="token number">1000000</span> <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h3 id="clustering-example">Clustering Example</h3>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">const</span> cluster <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'cluster'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">const</span> os <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'os'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">if</span> <span class="token punctuation">(</span>cluster<span class="token punctuation">.</span>isMaster<span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">const</span> numCPUs <span class="token operator">=</span> os<span class="token punctuation">.</span><span class="token function">cpus</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>length<span class="token punctuation">;</span>
  <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">let</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> i <span class="token operator">&lt;</span> numCPUs<span class="token punctuation">;</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
    cluster<span class="token punctuation">.</span><span class="token function">fork</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span> <span class="token keyword">else</span> <span class="token punctuation">{</span>
  <span class="token keyword">const</span> express <span class="token operator">=</span> <span class="token function">require</span><span class="token punctuation">(</span><span class="token string">'express'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">const</span> app <span class="token operator">=</span> <span class="token function">express</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

  app<span class="token punctuation">.</span><span class="token keyword">get</span><span class="token punctuation">(</span><span class="token string">'/'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span>req<span class="token punctuation">,</span> res<span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span>
    res<span class="token punctuation">.</span><span class="token function">send</span><span class="token punctuation">(</span><span class="token template-string"><span class="token string">`Handled by worker </span><span class="token interpolation"><span class="token interpolation-punctuation punctuation">${</span>process<span class="token punctuation">.</span>pid<span class="token interpolation-punctuation punctuation">}</span></span><span class="token string">`</span></span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

  app<span class="token punctuation">.</span><span class="token function">listen</span><span class="token punctuation">(</span><span class="token number">3000</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<h3 id="microservices">Microservices</h3>
<p>Complex tasks can be offloaded to separate services, improving scalability.</p>
<hr>
<h2 id="key-takeaways-5">Key Takeaways</h2>
<ul>
<li><strong>Event Loop</strong> ensures non-blocking I/O by delegating tasks to the thread pool or OS.</li>
<li>Use <strong>Worker Threads</strong> or <strong>Clustering</strong> for CPU-bound tasks to avoid blocking the event loop.</li>
<li>Node.js is designed for <strong>high-concurrency applications</strong>, making it efficient for handling many simultaneous I/O-bound requests.</li>
</ul>

