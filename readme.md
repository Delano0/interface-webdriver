UI automated testing framework powered by [Node.js](http://nodejs.org/).
Uses the [Selenium WebDriver API](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol).
Uses the [ChromeDriver API](https://chromium.googlesource.com/chromium/src/+/master/docs/chromedriver_status.md)

Base: lazy elements, chrome driver dirrect connection, own standalone server and chrome driver installer

Install [Node.js](http://nodejs.org/) and install framework


## Improvement plan
 * [x] Run selenium server from a client instance method (now only for OSX* systems)
 * [x] Add possibility find element by xpath (done)
 * [ ] Develop error handler system 
 * [ ] Develop and improve enviroment installer for every OS (unix, windows)


```sh
$ npm i --SD wd-interface
```
Drivers installation (now only for OSX* systems)
```sh
$ wd-interface standalone chrome gecko
```
Run driver selenium-standalone-server or chrome driver (now only for OSX* systems)
```sh
$ wd-interface start standalone  #for standalone
$ wd-interface start chrome  #for chromedriver 
$ wd-interface start gecko  #for geckdriver
```

Simple as a library

Use with [mocha](https://mochajs.org/) or other test runner 

# Base example [Api](#api)

```js
const { expect } = require('chai')

const { client, element } = require('wd-interface')

describe('Google base example', () => {
  let browser = null
  const baseURL = 'https://www.google.com.ua/'

  //selectors
  const submitsearch = '[name="btnK"]'
  const inputsearch = '#lst-ib'
  const resultsearch = '#ires .g'
  //elements
  const submitSearch = element(submitsearch)
  const inputSearch = element(inputsearch)
  const resultSearch = element(resultsearch)

  before(async () => {
    browser = client().chrome() // for dirrect connection to chromedriver client().chrome(true)
    await browser.goTo(baseURL)
  })

  after(async () => {
    await browser.closeBrowser()
  })

  it('search git hub potapovDim', async () => {
    await inputSearch.sendKeys('git hub potapovDim')
    await submitSearch.click()
    await browser.sleep(1000)
    await resultSearch.waitForElement(1000)
    const allTextInSelector = await resultSearch.getText()
    expect(allTextInSelector).to.includes('potapovDim')
  })
})
```

## Api
- [Browser](#browser)
  * [chrome](#chrome)
  * [goTo](#goto)
  * [closeCurrentTab](#closecurrenttab)
  * [getTitle](#gettitle)
  * [executeScript](#executescript)
  * [switchToTab](#switchtotab)
  * [closeBrowser](#closebrowser)
  * [getCurrentBrowserTab](#getcurrentbrowsertab)
  * [getBrowserTabs](#getbrowsertabs)
  * [sleep](#sleep)
  * [getUrl](#geturl)
- [Elements](#elements)
  * [map](#map)
  * [forEach](#foreach)
  * [filter](#filter)
  * [get](#get)
- [Element](#element)
  * [Constructor](#consctructor-element)
  * [sendKeys](#sendkeys)
  * [cliear](#clear)
  * [getElementHTML](#getelementhtml)
  * [waitForElementPresent](#waitforelementpresent)
  * [waitForElementVisible](#waitforelementvisible)
  * [getText](#gettext)
  * [waitForElement](#waitforelement)
  * [getElement](#getelement)
  * [getElements](#getelements)
  * [getAttribute](#getattribute)
  * [click](#click)
  * [isPresent](#ispresent)
  * [toElement](#toelement)
  * [isDisplayed](#isdisplayed)
  * [mouseDownAndMove](#mousedownandmove)

# Browser
```js
  const {client} = require('wd-interface')
  const browser = client.chrome() 

  /* return browser api instance
   * args directConnect
   * true - conncect direct to chromedriver port 9515 without selenium standalone
   * any, false , undefined - conncect to standalone server port 4444
   * /
```
## goTo
```js
  const browser = client.chrome() 
  await browser.goTo('https://google.com')
  /* args url
   * type string
   * /
```
## closeCurrentTab
```js
  const browser = client.chrome() 
  await browser.closeCurrentTab()
  /* 
   * will close current tab 
   * if opened tabs length more than 1
   * /
```
## getTitle
```js
  const browser = client.chrome() 
  const currentTitle = await browser.getTitle()
  /* 
   * will return tab title
   * /
```
## executeScript
```js
  const browser = client.chrome() 
  const currentTitle = await browser.executeScript(function () {
    const [cssSelector] = arguments
    return document.querySelector(cssSelector).innerHTML
  }, 'body')
  /* first arg is function or string function ,for example 'return arguments[0]'
   * if function return value it will be returned
   * /
```
## switchToTab
```js
  const browser = client.chrome() 
  await browser.switchToTab(1)
  /* for example if was opened link with _blank
   * will switch to opened tab
   * /
```
## closeBrowser
```js
  const browser = client.chrome() 
  await browser.closeBrowser()
  /* for example if was focused tab from switchToTab example
   * this will close current tab and focus you to previous
   * /
```
## getCurrentBrowserTab
```js
  const browser = client.chrome() 
  const tabId = await browser.getCurrentBrowserTab()
  /* return selenium tab id * /
```
## getBrowserTabs
```js
  const browser = client.chrome() 
  const tabIdS = await browser.getBrowserTabs()
  /*
  * return array with selenium tab ids
  */
```
## sleep
```js
  const browser = client.chrome() 
  await browser.sleep(1000)
  /* args number timeout 
  * will wait until timeout end
  */
```
## getUrl
```js
  const browser = client.chrome() 
  const currentUrl = await browser.getUrl()
  /* return current tab url*/
```
# Element
## Constructor element
```js
  const elementDiv = element('div')
  /* 
  * args css selector for example '#id', '[name="name"]', '.class'
  */
```
## sendKeys
```js
  const elementInput = element('input')
  await elementInput.sendKeys('test value')
  /* 
  * args string or array string
  */
```
## clear
```js
  const elementInput = element('input')
  await elementInput.clear()
  /* 
  * clear value inside input
  */
```
## getElementHTML
```js
  const elementInput = element('input')
  const inputHTML = await elementInput.getElementHTML()
  /* 
  * return outerHTML of current element , return string
  * <input value="a"/> for example
  */
```
## getText
```js
  const elementDiv = element('div')
  const divText = await elementDiv.getText()
  /* 
  * return all text inside element , return string
  */
```
## waitForElement
```js
  const elementDiv = element('div')
  await elementDiv.waitForElement(1000)
  /* 
  * will wait for element mount to DOM node
  */
```
## waitForElementPresent
```js
  const elementDiv = element('div')
  await elementDiv.waitForElementPresent(1000)
  /* 
  * will wait for element mount to DOM node
  */
```
## waitForElementVisible
```js
  const elementDiv = element('div')
  await elementDiv.waitForElementVisible(1000)
  /* 
  * will wait for element visible in DOM node
  */
```
## getElement
```js
  const elementSpan = element('div').getElement('span').getElement('a')
  /* 
  *  return element instanse
  */
```
## getElements
```js
  const elementsSpan = element('div').getElements('span')
  /* 
  *  return Elements instance
  */
```
## getAttribute
```js
  const elementSpan = element('div').getElement('span')
  const style = await elementSpan.getAttribute('style')
  /*
  * args strin , value name , for example 'value', 'href', 'style' etc 
  *  return string of attribute value
  */
```
## click
```js
  const elementSpan = element('div').getElement('span')
  await elementSpan.click()
  /*
  * triger click 
  */
```
## isPresent
```js
  const elementSpan = element('div').getElement('span')
  const present = await elementSpan.isPresent()
  /*
  * return true if element mounted to DOM
  * return false if element didn`t mount to DOM
  */
```
## isDisplayed
```js
  const elementSpan = element('div').getElement('span')
  const display = await elementSpan.isDisplayed()
  /*
  * return true if visible and in view port
  * return false if doesn`t visible, for example display: none
  */
```
## toElement
```js
  const elementSpan = element('div').getElement('span')
  await elementSpan.toElement()
  /*
  * will scroll view port to element 
  */
```
## mouseDownAndMove
```js
  const elementSpan = element('div').getElement('span')
  await elementSpan.mouseDownAndMove({x: 100, y: 0})
  /*
  * args object with x and y 
  * will mouse down mouse move from x and y from arg
  */
```
# Elements
```js
const {elements} = require('wd-interface')
// by css selector
const elementsSpan = elements('span')
// by xpath 
const elementsDiv = elements('xpath: /html/body/div[1]/div/div/div/div[1]/div[2]/div[2]/div/div')
 /* 
  *  args css selector or xpath format xpath: /html/body/div (for example)
  *  return array Element instaces
  */
```
### map
```js
  // by css selector
  const elementsSpan = elements('span')
  const textArr = await elementsSpan.mappy(async (element) => {
        return await element.getText()
      })
  /* 
  *  args async call back 
  *  return array  
  */
```
### forEach
```js
  //by css selector
  const elementsSpan = elements('span')

  const textArr = await elementsSpan.forEach(async (element) => {
        const html = await element.getElementHTML()
        expect(html).to.includes('dropitem')
        expect(html).to.includes('draggable="true"')
      })
  /* 
  * args async call back 
  * call async funcs with await 
  * does not return 
  * /
```
### filter
```js
 const elementsSpan = elements('span')
 const textArr = await elementsSpan.filter(async (element) => {
        const html = await element.getElementHTML()
        return html.includes('class="test"')
      })
  /* 
  * args async call back 
  * call async funcs with await
  * return new elements array  
  * /
```
### get
```js
 const elementsSpan = elements('span')
 const textArr = await elementsSpan.get(0)
  /* 
  * args index number
  * return Element instance 
  * /
```