# Playwright Introduction
<details>
  
* Playwright is a framework for Web Testing and Automation. It allows testing Chromium, Firefox and WebKit with a single API. Playwright is built to enable cross-browser web automation that is ever-green, capable, reliable and fast. Headless execution is supported for all browsers on all platforms.

![image](https://github.com/user-attachments/assets/957f006f-5a1d-4236-a34c-afaa2d148f7e)

* As Playwright is written by the creators of the Puppeteer, you would find a lot of similarities between them.
* Playwright has its own test runner for end-to-end tests, we call it Playwright Test.
* Cross-browser. Playwright supports all modern rendering engines including Chromium, WebKit, and Firefox.
* Cross-platform. Test on Windows, Linux, and macOS, locally or on CI, headless or headed.
* Cross-language. Use the Playwright API in TypeScript, JavaScript, Python, .NET, Java. The core framework is implemented using TypeScript.
* Playwright development is sponsored by Microsoft.

![image](https://github.com/user-attachments/assets/9b0a5d6e-7e9d-44a6-ad92-0a8069dba669)

[GitHub](https://github.com/microsoft/playwright)
[Documentation](https://playwright.dev/docs/intro)
[API reference](https://playwright.dev/docs/api/class-playwright/)
[Changelog](https://github.com/microsoft/playwright/releases)

</details>

# Playwright vs Cypress vs Puppeteer: Key Similaries

<details>
  
![image](https://github.com/user-attachments/assets/5c0a7e15-ba25-408a-9f35-e7f5a72bae0b)

![image](https://github.com/user-attachments/assets/3ee5c2a6-e48a-4801-a510-d9bb22bb761e)

![image](https://github.com/user-attachments/assets/38068e3e-6898-49fa-9e36-478710b39bb7)

![image](https://github.com/user-attachments/assets/b81b65c4-2c89-4425-9695-9d4e31394838)

</details>

# Playwright Architecture
<details>
  
![image](https://github.com/user-attachments/assets/7577f21c-e6de-4453-adb6-f1ee5c6c8f85)

![image](https://github.com/user-attachments/assets/d851112c-7cc0-4af3-94a2-cc61acdcd70b)

![image](https://github.com/user-attachments/assets/c7ec77f6-bd4b-4ea0-bb3b-983dc065af9d)

</details>

# Playwright Main Parts

![image](https://github.com/user-attachments/assets/acb4d541-2a32-4520-88d9-b239e67fde5f)

# Playwright Library
## Browser -> Browser Context -> Page
<details>
  
### Browser
Each version of Playwright needs specific versions of browser binaries to operate. Depending on the language you use, Playwright will either download these browsers at package install time for you, or you will need to manually use Playwright CLI install these browsers. Support Chromium, Firefox, WebKit, Browser Stable/Beta Channels (Google Chrome, Microsoft Edge)

https://playwright.dev/docs/browsers 

- browser.on(‘disconnected’)
- browser.close()
- browser.contexts()
- browser.isConnected()
- browser.newBrowserCDPSession()
- browser.newContext([options])
- browser.newPage([options])
- browser.startTracing([page, options])
- browser.stopTracing()
- browser.version()

https://playwright.dev/docs/api/class-browser 

### BrowserContext
- A BrowserContext is an isolated incognito-alike session within a browser instance.
- Comparing to instantiating browser for every test cases, it is faster and cheaper to create a new browser context
- A Browser can create multiple Browser Contexts
- A Browser Context can create multiple Pages
- Can be used to emulate multi-page scenarios involving mobile devices, permissions, locale, color scheme

How to create a new browser context? 

![image](https://github.com/user-attachments/assets/2181b209-f7bf-42e0-afb4-40eb21158c5d)

https://playwright.dev/docs/api/class-browsercontext 
https://playwright.dev/docs/browser-contexts

Tips for BrowserContext
- (x) Never Restart a Brower
  - (x) Slow instantiation (>100 ms)
  - (x) Huge memory overhead

- (v) Always create Browser Contexts
  - (v) Full isolation
  - (v) Fast instantiation (~1ms)
  - (v) Low overhead

### Page
A Page refers to a single tab or a popup window within a browser context
Page is used to navigate to URLs and interact with the page content
Notes: the URL must include the scheme HTTP/HTTPS.

![image](https://github.com/user-attachments/assets/93d0c61e-cf97-433f-9ef5-7b29c399b9f0)

https://playwright.dev/docs/pages 
https://playwright.dev/docs/api/class-page 
</details>

## Playwright Selector

![image](https://github.com/user-attachments/assets/1a78bc43-d74b-47ca-974a-197ab3910177)

https://playwright.dev/docs/locators

<details>
  
### Locators
Locators represent a way to find element(s) on the page at any moment and have auto-waiting and retry-ability. These are the recommended built-in locators. 

```
page.getByRole() to locate by explicit and implicit accessibility attributes.
page.getByText() to locate by text content.
page.getByLabel() to locate a form control by associated label's text.
page.getByPlaceholder() to locate an input by placeholder.
page.getByAltText() to locate an element, usually image, by its text alternative.
page.getByTitle() to locate an element by its title attribute.
page.getByTestId() to locate an element based on its data-testid attribute (other attributes can be configured).
page.locator() to locate an element based on CSS and XPath selectors
```

NOTE: Every time a locator is used for an action, an up-to-date DOM element is located in the page
```
const locator = page.getByRole('button', { name: 'Sign in' });

await locator.hover(); // located 1st
await locator.click(); // relocated 2nd
```

#### Locate by role
Refer `role` value in https://www.w3.org/TR/html-aria/#docconformance - Rules of ARIA attribute usage by HTML element
```
await expect(page.getByRole('heading', { name: 'Sign up' })).toBeVisible();
await page.getByRole('checkbox', { name: 'Subscribe' }).check();
await page.getByRole('button', { name: /submit/i }).click();
```

#### Locate by label
Use this locator when locating **form** fields.
```
<label>Password <input type="password" /></label>

await page.getByLabel('Password').fill('secret');
```

#### Locate by placeholder
Use this locator when locating **form** elements that do not have labels but do have placeholder texts.
```
<input type="email" placeholder="name@example.com" />

await page
    .getByPlaceholder('name@example.com')
    .fill('playwright@microsoft.com');
```

#### Locate by text

```
//You can locate the element by the text it contains:
await expect(page.getByText('Welcome, John')).toBeVisible();

//Set an exact match:
await expect(page.getByText('Welcome, John', { exact: true })).toBeVisible();

//Match with a regular expression:
await expect(page.getByText(/welcome, [A-Za-z]+$/i)).toBeVisible();
```

#### Locate by alt text
Use this locator when your element supports alt text such as `img` and `area` elements.
```
<img alt="playwright logo" src="/img/playwright-logo.svg" width="100" />

await page.getByAltText('playwright logo').click();
```

#### Locate by title

```
<span title='Issues count'>25 issues</span>

await expect(page.getByTitle('Issues count')).toHaveText('25 issues');
```

#### Locate by test id
QA's and developers should define explicit and unchangable test ids and query them. By default, `page.getByTestId()` will locate elements based on the `data-testid` attribute, but you can configure it in your test config or by calling `selectors.setTestIdAttribute()`.
```
<button data-testid="directions">Itinéraire</button>

await page.getByTestId('directions').click();
```

```
//playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  use: {
    testIdAttribute: 'data-pw'
  }
});
```

#### Locate by CSS or XPath
```
await page.locator('css=button').click();
await page.locator('xpath=//button').click();

await page.locator('button').click();
await page.locator('//button').click();
```

#### Locate in Shadow DOM
All locators in Playwright by default work with elements in Shadow DOM. The exceptions are:

- Locating by **XPath** does not pierce shadow roots.
- Closed-mode shadow roots are not supported.

```
<x-details role=button aria-expanded=true aria-controls=inner-details>
  <div>Title</div>
  #shadow-root
    <div id=inner-details>Details</div>
</x-details>

await page.getByText('Details').click();
await page.locator('x-details', { hasText: 'Details' }).click();
```

#### Filtering Locators
```
//Filter by text
await page
    .getByRole('listitem')
    .filter({ hasText: 'Product 2' }) // can pass regular expression  .filter({ hasText: /Product 2/ })
    .getByRole('button', { name: 'Add to cart' })
    .click();

//Filter by not having text
await expect(page.getByRole('listitem').filter({ hasNotText: 'Out of stock' })).toHaveCount(5);
```

```
<ul>
  <li>
    <h3>Product 1</h3>
    <button>Add to cart</button>
  </li>
  <li>
    <h3>Product 2</h3>
    <button>Add to cart</button>
  </li>
</ul>

//Filter by child/descendant
await page
    .getByRole('listitem') // li
    .filter({ has: page.getByRole('heading', { name: 'Product 2' }) }) // find child/descendant h1-h6, is queried starting with the original 'li' locator match
    .getByRole('button', { name: 'Add to cart' })
    .click();

//Filter by not having child/descendant
await expect(page
    .getByRole('listitem')
    .filter({ hasNot: page.getByText('Product 2') }))
    .toHaveCount(1);
```

#### Locator operators
* Matching
```
//Matching inside a locator
const product = page.getByRole('listitem').filter({ hasText: 'Product 2' });
await product.getByRole('button', { name: 'Add to cart' }).click();

const saveButton = page.getByRole('button', { name: 'Save' });
const dialog = page.getByTestId('settings-dialog');
await dialog.locator(saveButton).click();

//Matching two locators simultaneously
const button = page.getByRole('button').and(page.getByTitle('Subscribe'));

//Matching one of the two alternative locators
//e.g: you'd like to click on a "New email" button, but sometimes a security settings dialog shows up instead. In this case, you can wait for either a "New email" button, or a dialog and act accordingly.
//If both "New email" button and security dialog appear on screen, the "or" locator will match both of them, possibly throwing the "strict mode violation" error. In this case, you can use locator.first() to only match one of them.
const newEmail = page.getByRole('button', { name: 'New' });
const dialog = page.getByText('Confirm security settings');
await expect(newEmail.or(dialog).first()).toBeVisible();
if (await dialog.isVisible())
  await page.getByRole('button', { name: 'Dismiss' }).click();
await newEmail.click();

//Matching only visible elements
<button style='display: none'>Invisible</button>
<button>Visible</button>

await page.locator('button').locator('visible=true').click();
```

* Lists
```
//Count items in a list
await expect(page.getByRole('listitem')).toHaveCount(3);

//Assert all text in a list
await expect(page
    .getByRole('listitem'))
    .toHaveText(['apple', 'banana', 'orange']);

//Get a specific item
const banana = await page.getByRole('listitem').nth(1); //Get by nth item

//Chaining filters
const rowLocator = page.getByRole('listitem')
    .filter({ hasText: 'Mary' })
    .filter({ has: page.getByRole('button', { name: 'Say goodbye' }) })
    .screenshot({ path: 'screenshot.png' });

//Iterate elements
for (const row of await page.getByRole('listitem').all())
  console.log(await row.textContent());
```

- Select elements matching one of the conditions
```
await page.locator('button:has-text("Log in"), button:has-text("Sign in")').click(); => Click a button that has either a “Log in” or “Sign in” text
await page.locator(`//span[contains(@class, 'spinner__loading')] | //div[@id='confirmation']`).waitFor(); => Wait for either confirmation dialog or the loading indicator to appear
```


#### Selector - Layout
Select element by Page Layout: 
Layout selectors use bounding client rect to compute distance and relative position of the elements.
```
:right-of(inner > selector) - Matches elements that are to the RIGHT of any element matching the inner selector, at any vertical position.
:left-of(inner > selector) - Matches elements that are to the LEFT of any element matching the inner selector, at any vertical position.
:above(inner > selector) - Matches elements that are ABOVE any of the elements matching the inner selector, at any horizontal position.
:below(inner > selector) - Matches elements that are BELOW any of the elements matching the inner selector, at any horizontal position.
:near(inner > selector) - Matches elements that are NEAR (within 50 CSS pixels) any of the elements matching the inner selector.
```
Example: `await page.locator('input:right-of(:text("Username"))').fill('value');`

![image](https://github.com/user-attachments/assets/fdbe84b6-72a3-415f-8ab4-1c6a9b17ce03)

https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect 

#### Selector - Shadow DOM

![image](https://github.com/user-attachments/assets/9c90c125-1827-463f-bb5a-8d324930a3e5)

https://kipalog.com/posts/Duc-khoet-Javascript--Phan-17---ben-trong-Shadow-DOM---xay-dung-component-khep-kin 

![image](https://github.com/user-attachments/assets/661eca2d-9660-4626-96da-510d521dfbb0)

CSS and Text selector engines pierce the Shadow DOM by default:
- First they search for the elements in the light DOM in the iteration order, and
- Then they search recursively inside open shadow roots in the iteration order.

To opt-out of the default Shadow DOM piercing feature, we can use:
`await page.locator(‘:light(.article > .header)’).click()`

![image](https://github.com/user-attachments/assets/c4b7cc7b-5006-4d03-b54c-e71ae4ced223)

#### Selector - React, Vue, Angular components
React selector:
```
Page.locator(‘_react=BookItem’)
Page.locator(‘BookItem’)
```

Vue selector:
```
Page.locator(‘_vue=book-item’)
Page.locator(‘book-item[author=“steven pinker”]’)
```

Angular selector:
```
Page.locator(‘[ng-model="todoList.todoText"]’)
Page.locator('[ng-repeat="todo in todoList.todos"]');
```

These selectors only work against unminified application builds

### Chaining Selector
Selectors defined as `engine=body` or in short-form can be combined with the `>>` token, e.g. `selector1 >> selector2 >> selectors3`. When selectors are chained, the next one is queried relative to the previous one's result
```
Page.locator(‘article >> css=.dragon > .ball >> role=button[disabled=true]) 
```
The above line is equivalent to
```
Document
  .querySelector(‘article’)
  .querySelector(‘.dragon > .ball’)
  .querySelector(‘button[disabled=true]’)
```
https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector 

### Custom Selector Engine
Selector engine should have the following properties:
- Create function to create a RELATIVE selector from ROOT (ROOT is either a Document, ShadowRoot or Element) to a target element.
- Query function to query FIRST element matching selector relative to the root.
- QueryAll function to query ALL elements matching selector relative to the root.
https://playwright.dev/docs/extensibility#custom-selector-engines

![image](https://github.com/user-attachments/assets/ffc81a38-385d-49f4-bf3d-25b8cf698b58)

### Selector Best Practices
- Prioritize user-facing attributes: If your website’s attributes like text content, placeholder, accessibility roles, labels RARELY changes
```
await page.locator('text="Login"').click();
await page.locator('css=[placeholder="Search GitHub"]').fill('query');
await page.locator('css=[aria-label="Close"]').click();
await page.locator('css=nav >> text=Login').click();
```

- Define explicit contract: When user-facing attributes changes FREQUENTLY
```
await page.locator('[data-test-id=directions]').click();
await page.locator('data-test-id=directions').click();
```

- Avoid selector tied to implementation:
```
// avoid long css or xpath chains
await page.locator('#tsf > div:nth-child(2) > div.A8SBwf > div.RNNXgb > div > div.a4bIc > input').click();
await page.locator('//*[@id="tsf"]/div[2]/div[1]/div[1]/div/div[2]/input').click();
```
- Three attributes of a good selector: Uniqueness, Stability, Conciseness

https://playwright.dev/docs/selectors#best-practices 

</details>

## Handle
<details>
  
### Handle - JSHandle
JSHandle represents an in-page JS object. It can be created by:
```
Const windowHandle = await page.evaluateHandle(() => ({ window, document });
Const windowHandle = await page.evaluateHandle(‘window’);
```
Example:
```
const tweetHandle = await page.$('.tweets');
expect(await tweetHandle.evaluate(node => node.innerText)).toBe('10 tweets');
```
```
jsHandle.asElement()
jsHandle.dispose()
jsHandle.evaluate(pageFunction, arg)
jsHandle.evaluateHandle(pageFunction[, arg])

jsHandle.getProperties()
jsHandle.getProperty(propertyName)
jsHandle.jsonValue()
```
https://playwright.dev/docs/api/class-jshandle 

### Handle - ElementHandle
ElementHandle represents an in-page DOM elements. ElementHandle is a JS Handle. 
It can be created by:
```
const handle = await page.$(‘text=Submit’); // Find an element in the DOM with “Submit” text. If multiple elements are found, takes the first element 
const handle = await page.$$(‘text=Submit’); // Find all elements in the DOM with “Submit” text
```
Example:
```
const tweetHandle = await page.$('.tweets');
expect(await tweetHandle.evaluate(node => node.innerText)).toBe('10 tweets');
```

```
elementHandle.$(selector)
elementHandle.$$(selector)
elementHandle.boundingBox()
elementHandle.check([options])
elementHandle.click([options])
elementHandle.fill(value, [options])
elementHandle.innerText()
elementHandle.isEnabled()
elementHandle.press(key[,options])
elementHandle.selectOption(values[, options])
elementHandle.waitForElementState(state[, options])
elementHandle.waitForSelector(selector[, options])
```
https://playwright.dev/docs/api/class-elementhandle

</details>

## Locator
<details>

Locators are the central piece of Playwright's auto-waiting and retry-ability. In a nutshell, locators represent a way/rule on how to find element(s) on the page at any moment. 

- Locators are strict
```
// Throws if there are several buttons in DOM:
await page.locator('button').click();

// Works because we explicitly tell locator to pick the first element:
await page.locator('button').first().click();

// Works because count knows what to do with multiple matches – return number of elements:
await page.locator('button').count();
```
https://playwright.dev/docs/locators 
https://playwright.dev/docs/api/class-locator 

- Filter Locator
```
// Select element with text
Await page.locator(‘button’, { hasText: ‘Sign Up’ })

// Select elements that have a descendant matching another locator
Await page.locator(‘article’, { has: page.locator(‘button.subscribe’) }

// Filter an existing locator
const buttonLocator = page.locator('button');
// Do something, AJAX executing, DOM changes … 
await buttonLocator.filter({ hasText: 'Sign up' }).click();
```

- Frame Locator

![image](https://github.com/user-attachments/assets/b8b185ad-09fe-4d81-b679-a93f8f6e317d)

- Locator vs ElementHandle

![image](https://github.com/user-attachments/assets/b6aafa8a-9bfb-4011-bb38-e96e5935dd92)

</details>

## Wait
<details>
Before performing any action to interact with the page, we normally have to wait until the element we desired satisfied some conditions (visible, enabled,…). Playwright offers multiple wait strategies to resolve flakiness:

**Hard wait**
```
await page.waitFor(1000); // wait for 1 second
// or page.waitForTimeout(1000);
await page.click(‘#loginBtn’);
```

![image](https://github.com/user-attachments/assets/470f25ee-9d2e-4d03-abc6-8ec6290d7fde)

**Explicit wait**
- Wait until page satisfied navigation:

![image](https://github.com/user-attachments/assets/5145382d-53e2-4ce5-8d52-25c55b160bf0)

```
page(frame).waitForNavigation(); //Wait until page navigation completed
page(frame).waitForLoadState(); //Wait until the required load state has been reached
page(frame).waitForURL(); //Wait until a navigation to the target URL
```
All above APIs default to waiting for the load event. Below are list of navigation events we can wait:
```
commit //Wait until network response is received, document start loading
domcontentloaded //Wait until DomContentLoaded event is fired 
load (default) //Wait until load event is fired
networkidle //Wait until there are no network conditions for at least 500ms
```

 ![image](https://github.com/user-attachments/assets/de9932a9-d717-451e-ae3e-56e3e84682a0)

- Wait until page satisfied network conditions:
```
page(frame).waitForRequest(); //Wait until a specified request is sent out
page(frame).waitForLoadState(); //Wait until a specified response is received
```
```
page(frame).waitForSelector(selector);
page(frame).waitForNavigation();
page(frame).waitForEvent(event);
page(frame).waitForFunction(pageFunc);
page(frame).waitForLoadState([state, options]);
page(frame).waitForRequest(urlOrPredicate);
page(frame).waitForResponse(urlOrPredicate);
page(frame).waitForURL();
locator.waitFor([options]); // wait until element specified by the locator satisfied a state

elementHandle.waitForElementState(state);
elementHandle.waitForSelector(selector);
```
https://playwright.dev/search?q=waitFor 

**Auto wait (Built-in wait)**

Example, for page.click(selector[, options]) 

Playwright will ensure that:
- element is Attached to the DOM
- element is Visible
- element is Stable, as in not animating or completed animation
- element Receives Events, as in not obscured by other elements
- element is Enabled

If the required checks do not pass within the given timeout, action fails with the TimeoutError.

![image](https://github.com/user-attachments/assets/d5eea608-bac7-4b94-a0de-f7bd1b33e0e8)

https://playwright.dev/docs/actionability
https://playwright.dev/docs/input   

**Tips for waiting**

x Avoid using hard-wait
	
v Use Auto-Wait
v Use Explicit Wait when applicable

https://dev.to/checkly/avoiding-hard-waits-in-playwright-and-puppeteer-272 
</details>

## Network

<details>
  
![image](https://github.com/user-attachments/assets/43959073-f057-41a9-86a0-f396ec6c3db2)

https://playwright.dev/docs/network
https://playwright.dev/docs/api/class-playwright#playwright-request 
https://playwright.dev/docs/api/class-apirequest#api-request-new-context  

![image](https://github.com/user-attachments/assets/fea201b4-7fd5-4000-82a5-98bef755083d)

![image](https://github.com/user-attachments/assets/bc4f78cd-59ce-4be1-9b33-91588e5ecfa9)

![image](https://github.com/user-attachments/assets/298dca5d-6c51-4926-8bb4-c6b47f05238e)

</details>

## Page Object Model

<details>
  
![image](https://github.com/user-attachments/assets/efff5afc-7896-4e30-a7f2-08690d4265aa)

</details>

# Playwright Test
- A cross-browser test runner providing end-to-end testing for web apps. Features include browser automation for Playwright (Playwright API), Jest-like assertions and built-in support for TypeScript.
- Playwright Test makes use of the Microsoft Folio customizable test framework. Jest is a JavaScript testing framework, and Folio can be used to build your own test frameworks, and forms the foundation for the Playwright test runner. (https://github.com/microsoft/folio) 

![image](https://github.com/user-attachments/assets/25294469-2238-4c9a-abfe-3f4d9c90d19b)

## Assertion
<details>
  
### Web-first Assertion
  
![image](https://github.com/user-attachments/assets/179bad4a-6641-4457-9a36-bd398100ddfb)

![image](https://github.com/user-attachments/assets/ac246558-237b-41bf-9ad7-e05a7098c0f2)

```
await expect(locator).toBeChecked([options])
await expect(locator).toBeDisabled()
await expect(locator).toBeEnabled()
await expect(locator).toBeFocused()
await expect(locator).toBeHidden()
await expect(locator).toBeVisible()
await expect(locator).toContainText(expected[, opt])
await expect(locator).toHaveAttribute(name, value)
await expect(locator).toHaveCount(count, options)
await expect(locator).toHaveCSS(name, value, options)
await expect(locator).toHaveId(id, options) 
await expect(locator).toHaveScreenshot(options)
await expect(locator).toHaveText(expected, options)
 

await expect(page).toHaveURL(urlOrRegExp, options)
await expect(page).toHaveScreenshot(name, options)
await expect(page).toHaveTitle(titleOrRegExpOptions, options)
await expect(page).not
await expect(locator).not
await expect(screenshot).toMatchSnapshot(options)
```

https://playwright.dev/docs/test-assertions 
https://jestjs.io/docs/expect 
https://playwright.dev/docs/test-advanced#add-custom-matchers-using-expectextend (to extend the assertion function)

### Universal Retrying Assertion: expect.poll

![image](https://github.com/user-attachments/assets/2292ec1c-97df-48be-b299-7d9f0783e938)

### Tips for Assertion

x Avoid re-inventing the wheel with Universal Retrying Assertions
```
await expect.poll(async () => {
  const text = await page.locator(‘id=dialog’).textContent();
  return text;
}).toContain(‘Accept cookies’);
```

v Use Web-first Assertion instead
```
await expect(page.locator(‘id=dialog’)).toHaveText(‘Accept cookies’)
```

</details>

## Fixture
<details>

Playwright Test Fixture establishes environment for each test, giving the test everything it needs. It contains:
built-in fixtures for some basic features like BrowserContext, API Request, Environment configuration,…
Custom fixtures: like external JSON/CSV files, complex objects, page objects,…

The concept is borrowed from PyTest

https://playwright.dev/docs/test-fixtures 
https://playwright.dev/docs/api/class-fixtures 

- Fixture – Built-in fixture

![image](https://github.com/user-attachments/assets/fac1510c-ba84-4f39-8449-abb740bfd291)

![image](https://github.com/user-attachments/assets/021a8d2f-aa02-4c1e-a09d-b3483118c7ff)

- Page Object Model without Fixture: we manually initialize page object model in the test function (e.g: `ToDoPage`)

![image](https://github.com/user-attachments/assets/cdf8a630-831d-4e16-b89a-ae1602b5ce78)

- Page Object Model with Fixture

![image](https://github.com/user-attachments/assets/afea8cb3-28b3-4bd8-ab34-d5ec60b89e9c)

- Fixture – Advantages

Fixtures have a number of advantages over before/after hooks:

  - Fixtures **ENCAPSULATE** setup and teardown in the same place so it is easier to write.
  - Fixtures are **REUSABLE** between test files - you can define them once and use in all your tests. That's how Playwright's built-in page fixture works.
  - Fixtures are **ON-DEMAND** - you can define as many fixtures as you'd like, and Playwright Test will setup only the ones needed by your test and nothing else.
  - Fixtures are **COMPOSABLE** - they can depend on each other to provide complex behaviors.
  - Fixtures are **FLEXIBLE**. Tests can use any combinations of the fixtures to tailor precise environment they need, without affecting other tests.
  - Fixtures simplify **GROUPING**. You no longer need to wrap tests in describes that set up environment, and are free to group your tests semantically (by their meaning) instead.
  - Fixture are only initialized when required by test

- Fixture – Override fixture

![image](https://github.com/user-attachments/assets/b2d732fe-a5e9-421f-84ed-47daaedd0cb8)

Overriding page class, automatically navigating to baseUrl whenever the page class is instantiated.

- Fixture - Composable

![image](https://github.com/user-attachments/assets/0e35e2e8-6d92-41ca-a22f-2365a6b59639)

</details>

# Playwright Config
<details>
  
Playwright Test provides options to configure the default browser, context and page fixtures. For example there are options for headless, viewport and ignoreHTTPSErrors. You can also record a video or a trace for the test or capture a screenshot at the end.
https://playwright.dev/docs/test-configuration 
https://playwright.dev/docs/test-advanced 

![image](https://github.com/user-attachments/assets/2636ba67-d00c-4dbb-b9ac-dca1dc48af4d)

## GlobalConfig

![image](https://github.com/user-attachments/assets/145a0348-d27c-458a-aa78-058e657011fa)

## Main structure

![image](https://github.com/user-attachments/assets/1b6d71ec-f8b4-4e8f-9602-86d166922728)

## LocalConfig
We can override the global configurations on a specific test files or a test.describe block by using test.use(options)

![image](https://github.com/user-attachments/assets/bb98d39c-68f3-4e6d-b5bd-b3903ce72816)

## ProjectsConfig

![image](https://github.com/user-attachments/assets/b43742b3-d6f5-435c-a616-3837bf8b8a96)

![image](https://github.com/user-attachments/assets/cde42125-38b7-49a1-ba0e-50bcf25e5ff5)

## TimeoutConfig

![image](https://github.com/user-attachments/assets/23da7199-363d-477b-b726-b64569f349da)


</details>

# Test Hook – Global Setup, Global Teardown
<details>
To set something up once before running ALL tests, use globalSetup option in the configuration file. Global setup file must export a single function that takes a config object. This function will be run once before all the tests.

Similarly, use globalTeardown to run something once after ALL the tests. You can pass data such as port number, authentication tokens, etc. from your global setup to your tests using environment variables.

![image](https://github.com/user-attachments/assets/0d06d24b-4cec-4a8d-9b7e-9ab17c84fc7d)

![image](https://github.com/user-attachments/assets/dedcbc2b-893e-4d83-ba40-dc81a0dd6838)
  
</details>

# Reporter
<details>

- Built-in reporter
  - Line (default)
  - Dot (default on CI)
  - List
  - HTML
  - JSON
  - Junit
  - Github Actions annotations
- Third party reporter
  - Allure
  - Tesults
- Custom reporter

![image](https://github.com/user-attachments/assets/87ae7a78-6f67-4682-9345-da0482a1f951)

![image](https://github.com/user-attachments/assets/4913155f-c6e8-4db5-930b-8fdbbceae339)

![image](https://github.com/user-attachments/assets/3a179836-cafc-4048-a272-ada874f2d8e0)

![image](https://github.com/user-attachments/assets/bae0c12b-2e04-4583-a743-08342be8d9ef)

## Custom Reporter

![image](https://github.com/user-attachments/assets/836a0e3f-b8ea-44f3-8a8c-153a4fda6ed5)

![image](https://github.com/user-attachments/assets/57e90824-9889-4fe7-82f2-e627aa0c332e)

</details>

# Playwright tools
## Playwright Inspector

![image](https://github.com/user-attachments/assets/ce5ba2ff-e612-466e-89aa-9ff587333b1d)


# Playwright - Framework

This is an automation framework using Playwright written in TypeScript.

## Framework Structure

```
non-bdd                                               //
├─ .editorconfig                                      //<This file is for unifying the coding style for different editors and IDEs
├─ .env                                               //<contains environment variables 
├─ .eslintignore                                      //<contains files, folders opted out from ESLint checking
├─ .eslintrc                                          //<contains ESLint rules
├─ .github                                            //
│  └─ workflows                                       //
│     └─ playwright.yml                               //<GitHub Actions file
├─ .gitignore                                         //<gitignore file
├─ __snapshots__                                      //<generated page snapshots
├─ azure-pipelines.yml                                //<Azure Pipeline file
├─ config                                             //<contains common configuration
│  ├─ default-screenshot-config.ts                    //<default screenshot configuration (capture full page, disable animations, etc)
│  └─ endpoints.ts                                    //<contains endpoints of the SUTs
├─ constants                                          //
│  ├─ api-constants.ts                                //<contains constant objects to be used in API Testing project
│  ├─ url-constants.ts                                //<contains constant objects of urls to be used in project
│  └─ placeholder-constants.ts                        //<a list of common placeholders to be used for generating test data (RANDOM_EMAIL, RANDOM_TEXT, etc)
├─ core                                               //<contains core helper functions, models, etc
│  ├─ api                                             //
│  │  ├─ api-request.ts                               //<Models for API request
│  │  └─ rest-client.ts                               //<REST API Client used for working with APIs
|  ├─ page                                            //
│  |  ├─ base.page.ts                                 //<Base Page Object 
|  |  └─ page.ts                                      //<contains general interface for page object
|  ├─ utils                                           //
|  ├─ csv-helper.ts                                   //<contains helper functions to handle CSV file (to be used for CSV data-driven test cases for example)
│  ├─ placeholder-helper.ts                           //<contains helper function to replace placeholders in the test data (placeholder pattern is defined in config/constants - $$PLACEHOLDER_PATTERN$$) 
│  └─ reflection-helper.ts                            //<contains helper functions to get class, object infos like recursively iterating and perform actions for each JSON object fields
├─ data                                               //<contains test data
│  ├─ auth.json                                       //<contains generated authenticated state (including session data, cookies, local storage) at test suite startup. this file can be consumed in test cases to bypass login operation - Applied for NOPCOMMERCE test suite
│  ├─ country-code.json                               //
│  ├─ frontend                                        //<Test data for NopCommerce page
│  │  └─ login-data.json                              //<Test data for NopCommerce Login page
├─ fixtures                                           //<contains Playwright fixtures
│  ├─ all.fixture.ts                                  //<contains combination of all fixtures
│  ├─ api.fixture.ts                                  //<contains all fixtures related to API Testing
│  ├─ base.fixture.ts                                 //<contains fixtures for Base classes (like base page object)
│  ├─ page.fixture.ts                                 //<contains fixtures related to page object
├─ helpers                                            //<contain helper functions, classes to work in some projects
│  ├─ api                                             //<contain helper functions, classes to work in API projects
│  │  ├─ account-api-helper.ts                        //<contain helper functions to work with BookStore Account API
│  |  ├─ api-header-helper.ts                         //<contain helper functions to work with BookStore Book API
│  |  └─ book-api-helper.ts                           //<contain helper functions to work with BookStore API
│  ├─ generators                                      //<contains test data generator functions
│  │  └─ text-generator.ts                            //<contain generator functions for text data type
├─ hooks                                              //
│  ├─ global-setup.ts                                 //globalSetup option file to set something up once before running all tests (for example: login to an account before running the test cases, seed test database with mock data, etc)
│  ├─ global-teardown.ts                              //<globalTeardown to run something once after all the tests (for example: reset user authenticated state, clear cookies, sessions, clean up test data in database etc)
│  ├─ setup-bookstore-api.ts                          //<functions for setting BookStore API test suite>
│  └─ setup-bookstore.ts                              //<functions for setting BookStore test suite>
├─ LICENSE                                            //
├─ models                                             //<business models of the SUT
│  ├─ business-models                                 //<business models of Bookstore page
   |  ├─ book                                         //
│  │  |  └─ login-info.ts                             //
│  │  └─ api                                          //
│  │     ├─ account-models.ts                         //
│  │     └─ book-models.ts                            //
├─ package-lock.json                                  //<provide an immutable version of package.json
├─ package.json                                       //<contains basic information about the project,registered dependencies and running script
├─ pages                                              //<contains page objects
│  ├─ bookdetail.page.ts                              //
│  ├─ bookstore.page.ts                               //
│  ├─ login.page.ts                                   //
│  ├─ profile.page.ts                                 //
├─ playwright.config.ts                               //<PlayWright configuration file
├─ README.md                                          //<Starting guideline
├─ reporters                                          //
│  ├─ custom_reporter.ts                              //<a sample for customized Playwright reporter
│  ├─ json-report-test-result.json                    //<a sample of generated json test report result 
│  └─ junit-report-test-result.xml                    //<a sample of generated JUnit test result
├─ tests                                              //
│  ├─ api                                             //<Test cases for BookStore API>
│  │  └─ get-user.spec.ts                             //
│  └─ book                                            //<Test cases for BookStore UI>
│     ├─ login.spec.ts                                //<Test cases of Bookstore Login feature applying POM + fixture
│     ├─ addbook.spec.ts                              //<Test cases of Bookstore Add Book feature applying POM + fixture
└─ tsconfig.json                                      //<The tsconfig.json file specifies the root files and the compiler options required to compile the project.

```

## Requirements

```
- Visual Code
- NodeJS version > 14 (Node.js 14 is no longer supported since it reached its end-of-life on April 30, 2023.)
- Playwright 1.32.3
```

# Getting Started

```
This is the quick and easy getting started assuming you already have git, Visual Code and NodeJS installed.
```

## Open project in Visual Code

```
- Launch Visual Code
- File -> Open Folder OR ctrl+K ctrl+O
- Select project root folder
```

## Install the required items

1. Install all required packages for project defined in the package.json file: Playwright, etc

```sh

Open Terminal window in Visual Code (ctrl + `) then execute command:
npm install

Or go to project root folder then open CMD windows and execute command:
npm install

```

2. Install Playwright Browsers

```sh

Open Terminal window in Visual Code (ctrl + `) then execute command:
npx playwright install

Or go to project root folder then open CMD windows and execute command:
npx playwright install

```

## Run Tests

### Run tests by Playwright VSCode extension

1. Install Playwright Test for VS Code extension on VS Code Marketplace (https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright)
2. You can run a single test by clicking the green triangle next to your test block to run your test. Playwright will run through each line of the test and when it finishes you will see a green tick next to your test block as well as the time it took to run the test.
![Run Test](https://user-images.githubusercontent.com/13063165/212735762-51bae36b-8c91-46f1-bd3a-24bd29f853d2.png)
3. You can also run your tests and show the browsers by selecting the option Show Browsers in the testing sidebar. Then when you click the green triangle to run your test the browser will open and you will visually see it run through your test. Leave this selected if you want browsers open for all your tests or uncheck it if you prefer your tests to run in headless mode with no browser open.
![Run Test](https://user-images.githubusercontent.com/13063165/212737059-0c52cda1-829d-4cda-9ca8-33741c87dfff.png)

   
### Run tests on Chrome/Firefox (include BookStore test suite)

```sh
For Chrome, Execute the command in the terminal: 
npm run test:bookstore-chromium

For Firefox, Execute the command in the terminal: 
npm run test:bookstore-firefox
```

### Run API tests only (include BookStore API test suite)

```sh
Execute the command in the terminal: 
npm run test:bookstore-api
```

### Run tests by feature tags (including BookStore Login test suite)

```sh
Execute the command in the terminal: 
npm run test:bookstore:login:chromium
```

### Run tests to update snapshots (including BookStore test suite)

Used for Visual Testing purpose
On the first run of the test suite, the framework will capture the screenshots of the pages and stored it in the __snapshots__ directory as Base Snapshots
On the second run of the test suite, the framework will compare the captured screenshots with the based screenshots (compare pixel by pixel under the hood)

```sh
To update the Base Snapshot for NopCommerce test suite, Execute the command in the terminal: 
npm run test:bookstore:update-snapshots
```

Please see the package.json file for more details (note the --update-snapshots command option)

### Run tests in parallel

We can run test cases in parallel in two ways

Option #1: Modify the "workers" field in the playwright.config.ts page -> this option will affect all test suites

Option #2: Add --workers arguments in the test run commands (only affect for specific test run)

```sh
Run Login test suite with 2 workers
npm run test:bookstore:login:chromium -- --workers=<number-of-workers>
```

For more details, please refer to Playwright document
[Playwright Parallelism and sharding](https://playwright.dev/docs/test-parallel)


### Generate Report

```
After running test complete, we can execute the following command in Visual Code Terminal window or CMD window:
npm run create:report

The HTML report will be generated in folder TestReport in root folder

We can change the type of reporter (JUnit, customized, 3rd party reporter - Allure, etc) in the playwright.config.ts file

Execute this command for generating Allure report:
npm run create:allure-report
```

### Run Linting to check coding convention of all projects (by ESLint)

```sh
Execute the command in the terminal: 
npm run lint
```

### How to configure and run tests on different environment or browser
Playwright has many options to configure how your tests are run. You can specify these options in the configuration file. Therefore, we can configure the enviroment which we use to run test in Project section of configuration file like below:

![Test Configuration](__snapshots__/Config.PNG)

In this sample, we configure 2 projects (1 for Chromium and 1 for Firefox) in order to run all tests on different browsers. We can also configure different baseURl for different Environments here.
Then, we can specify the scripts to run on multi environments on package.json file like below:
![Run Script](__snapshots__/scripts.PNG)

Finally, we can use npm run command to specify the enviroment that we want to run. For example, if we want to run all tests on Chromium we can use below command:
```sh
npm run test:bookstore-chromium
```
For more details, please refer to Playwright document
[Playwright Test Configuration](https://playwright.dev/docs/test-configuration)
