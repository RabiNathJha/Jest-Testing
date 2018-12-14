<!DOCTYPE html><html><head><meta charset="utf-8"><title>Unit Testing Using Jest:.md</title>
<h1><a id="Unit_Testing_Using_Jest_0"></a>Unit Testing Using Jest:</h1>
<h3><a id="What_is_Unit_testing_2"></a>What is Unit testing</h3>
<blockquote>
<p>UNIT TESTING is a level of software testing where individual units/ components of a software are tested.<br>
where <code>UNIT</code> = smallest testable part of any software</p>
</blockquote>
<h4><a id="Most_important_benefit_as_per_my_knowledge_5"></a>Most important benefit as per my knowledge</h4>
<ul>
<li>Codes are more reusable. In order to make unit testing possible, codes need to be modular. This means that codes are easier to reuse.</li>
<li>Will be able to detect bugs in first stage which leads to faster development.</li>
</ul>
<h6><a id="Letss_get_familiar_with_common_syntax_describe_and_it_9"></a>Lets’s get familiar with common syntax <code>describe()</code> and <code>it()</code>:</h6>
<p><code>describe()</code> : describe breaks your test suite into components.<br>
<code>it()</code> : it is where you perform individual tests. You should be able to describe each test like a little sentence, such as “should update state variables”</p>
<pre><code class="language-javascript">    describe('App Name', function () {
        it('test ....', function () {
        })'
    })
</code></pre>
<p><code>toBe()</code> and <code>toEqual()</code> [you will find yourself using this function a lot]:</p>
<p>For primitive types (e.g. numbers, booleans, strings, etc.), there is no difference between toBe and toEqual; either one will work for 5, true, or “the cake is a lie”</p>
<p>let’s take an example:</p>
<pre><code class="language-javascript"><span class="hljs-keyword">var</span> a = { bar: <span class="hljs-string">'baz'</span> },
    b = { foo: a },
    c = { foo: a };
    
&gt; b.foo.bar === c.foo.bar
<span class="hljs-literal">true</span>
&gt; b.foo.bar === a.bar
<span class="hljs-literal">true</span>
&gt; c.foo === b.foo
<span class="hljs-literal">true</span>
</code></pre>
<blockquote>
<p>But some things, even though they are “equal”, are not “the same”, since they represent objects that live in different locations in memory[<code>toBe()</code> is equal to ===].</p>
</blockquote>
<pre><code class="language-javascript">&gt; b === c
<span class="hljs-literal">false</span>
&gt; expect(b).toBe(c)
<span class="hljs-literal">false</span>
</code></pre>
<p><code>toEqual()</code>, which checks “deep equality” (i.e. does a recursive search through the objects to determine whether the values for their keys are equivalent).</p>
<pre><code class="language-javascript">&gt; expect(b).toEqual(c);
<span class="hljs-literal">true</span>
</code></pre>
<blockquote>
<p>There is a say in unit testing : If you are not mocking you not unit testing</p>
</blockquote>
<h4><a id="Mocking_using_jest_54"></a>Mocking using jest:</h4>
<h5><a id="There_are_three_main_types_of_module_and_function_mocking_in_Jest_56"></a>There are three main types of module and function mocking in Jest:</h5>
<ul>
<li><code>jest.fn</code>: Mock a function</li>
<li><code>jest.mock</code>: Mock a module</li>
<li><code>jest.spyOn</code>: Spy or mock a function</li>
</ul>
<p>Mock functions make it easy to test the links between code by erasing the actual implementation of a function.</p>
<h5><a id="Mock_functions_using_jestfn_64"></a>Mock functions using <code>jest.fn</code>:</h5>
<p>Let’s imagine we’re testing an implementation of a function forEach, which invokes a callback for each item in a supplied array.</p>
<pre><code class="language-javascript"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">forEach</span>(<span class="hljs-params">items, callback</span>) </span>{
  <span class="hljs-keyword">for</span> (<span class="hljs-keyword">let</span> index = <span class="hljs-number">0</span>; index &lt; items.length; index++) {
    callback(items[index]);
  }
}
</code></pre>
<p>To test this function, we can use a mock function, and inspect the mock’s state to ensure the callback is invoked as expected.</p>
<pre><code class="language-javascript"><span class="hljs-keyword">const</span> mockCallback = jest.fn(x =&gt; <span class="hljs-number">42</span> + x);
forEach([<span class="hljs-number">0</span>, <span class="hljs-number">1</span>], mockCallback);

<span class="hljs-comment">// The mock function is called twice</span>
expect(mockCallback.mock.calls.length).toBe(<span class="hljs-number">2</span>);

<span class="hljs-comment">// The first argument of the first call to the function was 0</span>
expect(mockCallback.mock.calls[<span class="hljs-number">0</span>][<span class="hljs-number">0</span>]).toBe(<span class="hljs-number">0</span>);

<span class="hljs-comment">// The first argument of the second call to the function was 1</span>
expect(mockCallback.mock.calls[<span class="hljs-number">1</span>][<span class="hljs-number">0</span>]).toBe(<span class="hljs-number">1</span>);

<span class="hljs-comment">// The return value of the first call to the function was 42</span>
expect(mockCallback.mock.results[<span class="hljs-number">0</span>].value).toBe(<span class="hljs-number">42</span>);
</code></pre>
<p><code>.mock property</code>:<br>
All mock functions have this special .mock property, which is where data about how the function has been called and what the function returned is kept.</p>
<blockquote>
<p>useful links:<br>
<a href="https://jestjs.io/docs/en/expect">https://jestjs.io/docs/en/expect</a><br>
<a href="https://jestjs.io/docs/en/mock-function-api">https://jestjs.io/docs/en/mock-function-api</a><br>
<a href="https://medium.com/@rickhanlonii/understanding-jest-mocks-f0046c68e53c">https://medium.com/@rickhanlonii/understanding-jest-mocks-f0046c68e53c</a></p>
</blockquote>
<p>More Examples:</p>
<pre><code class="language-javascript"><span class="hljs-comment">// memoize.js</span>
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">memoize</span>(<span class="hljs-params">func</span>) </span>{
  <span class="hljs-keyword">return</span> <span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-params">key</span>) </span>{
    <span class="hljs-keyword">const</span> registry = <span class="hljs-keyword">new</span> <span class="hljs-built_in">Map</span>();
    <span class="hljs-keyword">let</span> value = registry.get(key);
    <span class="hljs-keyword">if</span> (<span class="hljs-keyword">typeof</span> value === <span class="hljs-string">'undefined'</span>) {
      value = func(key);
      registry.set(key, value);
    }
    <span class="hljs-keyword">return</span> value;
  }
}
</code></pre>
<pre><code class="language-javascript"><span class="hljs-keyword">import</span> memoize <span class="hljs-keyword">from</span> <span class="hljs-string">'./memoize'</span>;

it(<span class="hljs-string">'memoizes `func`.'</span>, () =&gt; {
  <span class="hljs-comment">// A mock function: a function wrapped in `jest.fn`.</span>
  <span class="hljs-keyword">const</span> func = jest.fn(value =&gt; <span class="hljs-built_in">Math</span>.pow(value, <span class="hljs-number">3</span>));
  <span class="hljs-keyword">const</span> memoizedFunc = memoize(func);
  expect(memoizedFunc(<span class="hljs-number">3</span>)).toEqual(<span class="hljs-number">27</span>);
  expect(memoizedFunc(<span class="hljs-number">4</span>)).toEqual(<span class="hljs-number">64</span>);
  expect(memoizedFunc(<span class="hljs-number">3</span>)).toEqual(<span class="hljs-number">27</span>);
  expect(memoizedFunc(<span class="hljs-number">5</span>)).toEqual(<span class="hljs-number">125</span>);
  <span class="hljs-comment">// Mock functions have a few special matchers available to them.  Let's take a look.</span>
  <span class="hljs-comment">// `func` was called.</span>
  expect(func).toHaveBeenCalled();
  <span class="hljs-comment">// `func` has been called with 5.</span>
  expect(func).toHaveBeenCalledWith(<span class="hljs-number">4</span>);
  <span class="hljs-comment">// `func` was last called with 5.</span>
  expect(func).toHaveBeenLastCalledWith(<span class="hljs-number">5</span>);
  <span class="hljs-comment">// `func` was called thrice (since the second 3 was supposed to be memoized).</span>
  <span class="hljs-comment">// Supposed to.</span>
  expect(func).toHaveBeenCalledTimes(<span class="hljs-number">3</span>);
});
</code></pre>
<h5><a id="Mock_functions_using_jestmock_140"></a>Mock functions using <code>jest.mock</code>:</h5>
<blockquote>
<p>math.js</p>
</blockquote>
<pre><code class="language-javascipt">export const add      = (a, b) =&gt; a + b;
export const subtract = (a, b) =&gt; b - a;
export const multiply = (a, b) =&gt; a * b;
export const divide   = (a, b) =&gt; b / a;
</code></pre>
<blockquote>
<p>app.js</p>
</blockquote>
<pre><code class="language-javascript"><span class="hljs-keyword">import</span> * <span class="hljs-keyword">as</span> math <span class="hljs-keyword">from</span> <span class="hljs-string">'./math.js'</span>;

<span class="hljs-keyword">export</span> <span class="hljs-keyword">const</span> doAdd      = (a, b) =&gt; math.add(a, b);
<span class="hljs-keyword">export</span> <span class="hljs-keyword">const</span> doSubtract = (a, b) =&gt; math.subtract(a, b);
<span class="hljs-keyword">export</span> <span class="hljs-keyword">const</span> doMultiply = (a, b) =&gt; math.multiply(a, b);
<span class="hljs-keyword">export</span> <span class="hljs-keyword">const</span> doDivide   = (a, b) =&gt; math.divide(a, b);
</code></pre>
<p><code>jest.mock</code> to automatically set all exports of a module to the Mock Function. So, calling <code>jest.mock('./math.js');</code> essentially sets math.js to:</p>
<pre><code class="language-javascript"><span class="hljs-keyword">export</span> <span class="hljs-keyword">const</span> add      = jest.fn();
<span class="hljs-keyword">export</span> <span class="hljs-keyword">const</span> subtract = jest.fn();
<span class="hljs-keyword">export</span> <span class="hljs-keyword">const</span> multiply = jest.fn();
<span class="hljs-keyword">export</span> <span class="hljs-keyword">const</span> divide   = jest.fn();
</code></pre>
<p>Example on how to use <code>jest.mock</code> :</p>
<pre><code class="language-javascript"><span class="hljs-keyword">import</span> * <span class="hljs-keyword">as</span> app <span class="hljs-keyword">from</span> <span class="hljs-string">"./app"</span>;
<span class="hljs-keyword">import</span> * <span class="hljs-keyword">as</span> math <span class="hljs-keyword">from</span> <span class="hljs-string">"./math"</span>;

<span class="hljs-comment">// Set all module functions to jest.fn</span>
jest.mock(<span class="hljs-string">"./math.js"</span>);

test(<span class="hljs-string">"calls math.add"</span>, () =&gt; {
  app.doAdd(<span class="hljs-number">1</span>, <span class="hljs-number">2</span>);
  expect(math.add).toHaveBeenCalledWith(<span class="hljs-number">1</span>, <span class="hljs-number">2</span>);
});

test(<span class="hljs-string">"calls math.subtract"</span>, () =&gt; {
  app.doSubtract(<span class="hljs-number">1</span>, <span class="hljs-number">2</span>);
  expect(math.subtract).toHaveBeenCalledWith(<span class="hljs-number">1</span>, <span class="hljs-number">2</span>);
});
</code></pre>
<blockquote>
<p>The only disadvantage of this strategy is that it’s difficult to access the original implementation of the module. For those use cases, you can use spyOn.</p>
</blockquote>
<h5><a id="Spy_or_mock_a_function_with_jestspyOn__188"></a>Spy or mock a function with <code>jest.spyOn</code> :</h5>
<p>Sometimes you only want to watch a method be called, but keep the original implementation. Other times you may want to mock the implementation, but restore the original later in the suite.</p>
<p>In these cases, you can use jest.spyOn.</p>
<p>Here we simply “spy” calls to the math function, but leave the original implementation in place:</p>
<blockquote>
<p>math_spyOn.test.js</p>
</blockquote>
<pre><code class="language-javascript"><span class="hljs-keyword">import</span> * <span class="hljs-keyword">as</span> app <span class="hljs-keyword">from</span> <span class="hljs-string">"./app"</span>;
<span class="hljs-keyword">import</span> * <span class="hljs-keyword">as</span> math <span class="hljs-keyword">from</span> <span class="hljs-string">"./math"</span>;

test(<span class="hljs-string">"calls math.add"</span>, () =&gt; {
  <span class="hljs-keyword">const</span> addMock = jest.spyOn(math, <span class="hljs-string">"add"</span>);

  <span class="hljs-comment">// calls the original implementation</span>
  expect(app.doAdd(<span class="hljs-number">1</span>, <span class="hljs-number">2</span>)).toEqual(<span class="hljs-number">3</span>);

  <span class="hljs-comment">// and the spy stores the calls to add</span>
  expect(addMock).toHaveBeenCalledWith(<span class="hljs-number">1</span>, <span class="hljs-number">2</span>);
});
</code></pre>
<p>This is useful in a number of scenarios where you want to assert that certain side-effects happen without actually replacing them.</p>
<p><code>In other cases, you may want to mock a function, but then restore the original implementation</code>:</p>
<pre><code class="language-javascript"><span class="hljs-keyword">import</span> * <span class="hljs-keyword">as</span> app <span class="hljs-keyword">from</span> <span class="hljs-string">"./app"</span>;
<span class="hljs-keyword">import</span> * <span class="hljs-keyword">as</span> math <span class="hljs-keyword">from</span> <span class="hljs-string">"./math"</span>;

test(<span class="hljs-string">"calls math.add"</span>, () =&gt; {
  <span class="hljs-keyword">const</span> addMock = jest.spyOn(math, <span class="hljs-string">"add"</span>);

  <span class="hljs-comment">// override the implementation</span>
  addMock.mockImplementation(() =&gt; <span class="hljs-string">"mock"</span>);
  expect(app.doAdd(<span class="hljs-number">1</span>, <span class="hljs-number">2</span>)).toEqual(<span class="hljs-string">"mock"</span>);

  <span class="hljs-comment">// restore the original implementation</span>
  addMock.mockRestore();
  expect(app.doAdd(<span class="hljs-number">1</span>, <span class="hljs-number">2</span>)).toEqual(<span class="hljs-number">3</span>);
});
</code></pre>
<h4><a id="Snapshot_testing_228"></a>Snapshot testing:</h4>
<p>A typical snapshot test case for a mobile app renders a UI component, takes a screenshot, then compares it to a reference image stored alongside the test. The test will fail if the two images do not match: either the change is unexpected, or the screenshot needs to be updated to the new version of the UI component.</p>
<pre><code class="language-javascript"><span class="hljs-keyword">import</span> React <span class="hljs-keyword">from</span> <span class="hljs-string">'react'</span>;
<span class="hljs-keyword">import</span> Link <span class="hljs-keyword">from</span> <span class="hljs-string">'../Link.react'</span>;
<span class="hljs-keyword">import</span> renderer <span class="hljs-keyword">from</span> <span class="hljs-string">'react-test-renderer'</span>;

it(<span class="hljs-string">'renders correctly'</span>, () =&gt; {
  <span class="hljs-keyword">const</span> tree = renderer
    .create(<span class="xml"><span class="hljs-tag">&lt;<span class="hljs-title">Link</span> <span class="hljs-attribute">page</span>=<span class="hljs-value">"http://www.facebook.com"</span>&gt;</span>Facebook<span class="hljs-tag">&lt;/<span class="hljs-title">Link</span>&gt;</span>)</span>
    .toJSON();
  expect(tree).toMatchSnapshot();
});
</code></pre>
<pre><code class="language-javascript">exports[<span class="hljs-string">`renders correctly 1`</span>] = <span class="hljs-string">`
&lt;a
  className="normal"
  href="http://www.facebook.com"
  onMouseEnter={[Function]}
  onMouseLeave={[Function]}
&gt;
  Facebook
&lt;/a&gt;
`</span>;
</code></pre>
<p>But there is one problem that ‘react-test-renderer’ only does deep rendering which means child component’s will get called.</p>
<p>What if you do not want a snapshot test for you child commponents.<br>
In that case you can use <code>Shallow</code> from <code>enzyme</code> [<code>import { shallow } from 'enzyme'</code>] and also there is shallow renderer in jest.<br>
Example:</p>
<pre><code class="language-javascript">describe(<span class="hljs-string">'Component:CashflowPanel '</span>, () =&gt; {
    it(<span class="hljs-string">'should render correctly'</span>, () =&gt; {
        <span class="hljs-keyword">const</span> wrapper = shallow(<span class="xml"><span class="hljs-tag">&lt;<span class="hljs-title">CashflowPanel</span>
            <span class="hljs-attribute">data</span>=<span class="hljs-value">{cashflowdata}</span>
            <span class="hljs-attribute">isFetchException</span>=<span class="hljs-value">{false}</span>
            <span class="hljs-attribute">isFetched</span>=<span class="hljs-value">{true}</span>
            <span class="hljs-attribute">isFetching</span>=<span class="hljs-value">{false}</span>
        /&gt;</span>)</span>;
        expect(toJson(wrapper)).toMatchSnapshot();
    });
});
</code></pre>
<p>how to update snapshot : <code>jest --updateSnapshot</code></p>
<blockquote>
<p>difference between mount, shallow, render in enzyme:<br>
<a href="https://gist.github.com/fokusferit/e4558d384e4e9cab95d04e5f35d4f913">https://gist.github.com/fokusferit/e4558d384e4e9cab95d04e5f35d4f913</a></p>
</blockquote>
<blockquote>
<p>How to mock a module:<br>
<a href="https://www.leighhalliday.com/mocking-axios-in-jest-testing-async-functions">https://www.leighhalliday.com/mocking-axios-in-jest-testing-async-functions</a></p>
</blockquote>

</body></html>
