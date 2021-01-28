---
id: react-dom-server
title: ReactDOMServer
layout: docs
category: Reference
permalink: docs/react-dom-server.html
---

`ReactDOMServer` কম্পোনেন্টকে static মার্কআপে রেন্ডার করতে সাহায্য করে। এটি সাধারণত Node সার্ভারে ব্যবহৃত হয়ঃ

```js
// ES modules
import ReactDOMServer from 'react-dom/server';
// CommonJS
var ReactDOMServer = require('react-dom/server');
```

## সারমর্ম {#overview}

নিম্নোক্ত মেথডগুলো সার্ভার এবং ব্রাউজার উভয় ইনভায়রনমেন্টে ব্যবহৃত হয়ঃ

- [`renderToString()`](#rendertostring)
- [`renderToStaticMarkup()`](#rendertostaticmarkup)

এই অতিরিক্ত মেথডগুলো (`stream`) প্যাকেজের উপর নির্ভর করে যা **কেবল সার্ভারে কাজ করে**, ব্রাউজারে এগুলো কাজ করবে না।

- [`renderToNodeStream()`](#rendertonodestream)
- [`renderToStaticNodeStream()`](#rendertostaticnodestream)

* * *

## রেফারেন্স {#reference}

### `renderToString()` {#rendertostring}

```javascript
ReactDOMServer.renderToString(element)
```

এটি React element কে তার প্রাথমিক HTML এ রেন্ডার করে। React তখন একটি HTML string রিটার্ন করবে। আপনি এই মেথডটি ব্যবহার করে সার্ভারে HTML তৈরি করতে পারেন এবং দ্রুত পেজ লোড ও SEO এর উদ্দেশ্যে সার্চ ইঞ্জিনকে আপনার পেজটি crawl করার জন্য প্রাথমিক রিকোয়েস্টেই এই মার্কআপটি প্রদান করতে পারেন।

আপনি যদি [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate)কে এমন একটি node এ কল করেন, যার ইতিমধ্যে ওই server-rendered মার্কআপটি রয়েছে, React তখন সেটিকে সংরক্ষণ করবে এবং শুধু ইভেন্ট হেন্ডেলারগুলোকে সংযুক্ত করবে, যা আপনাকে খুবই কার্যকর একটি first-load অভিজ্ঞতা দিবে।

* * *

### `renderToStaticMarkup()` {#rendertostaticmarkup}

```javascript
ReactDOMServer.renderToStaticMarkup(element)
```

এটি [`renderToString`](#rendertostring) এর মতই, তবে React তার নিজ প্রয়োজনে যেসব অতিরিক্ত DOM attributes ব্যবহার করে, এটি তা তৈরি করে না, যেমন `data-reactroot`। যদি আপনি Reactকে static পেজ জেনারেটরের মত ব্যবহার করতে চান, তবে এটি খুব উপকারি, কারণ এটি অতিরিক্ত attributes মুছে ফেলে কিছু bytes বাঁচিয়ে দেয়।

আপনি যদি মার্কআপকে interactive করার জন্য ক্লায়েন্টে React ব্যবহার করার পরিকল্পনা করেন, তবে এই মেথডটি ব্যবহার করবেন না। এর পরিবর্তে সার্ভারে [`renderToString`](#rendertostring) এবং ক্লায়েন্টে [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) ব্যবহার করুন।

* * *

### `renderToNodeStream()` {#rendertonodestream}

```javascript
ReactDOMServer.renderToNodeStream(element)
```

এটি React element কে তার প্রাথমিক HTML এ রেন্ডার করে। এটি একটি [Readable stream](https://nodejs.org/api/stream.html#stream_readable_streams) রিটার্ন করে যেটি একটি HTML string আউটপুট দেয়। এই stream থেকে আউটপুট পাওয়া HTMLটি আসলে [`ReactDOMServer.renderToString`](#rendertostring) যা রিটার্ন করে তার অনুরূপ। আপনি এই মেথডটি ব্যবহার করে সার্ভারে HTML তৈরি করতে পারেন এবং দ্রুত পেজ লোড ও SEO এর উদ্দেশ্যে সার্চ ইঞ্জিনকে আপনার পেজটি crawl করার জন্য প্রাথমিক রিকোয়েস্টেই এই মার্কআপটি প্রদান করতে পারেন।

আপনি যদি [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate)কে এমন একটি node এ কল করেন, যার ইতিমধ্যে ওই server-rendered মার্কআপটি রয়েছে, React তখন সেটিকে সংরক্ষণ করবে এবং শুধু ইভেন্ট হেন্ডেলারগুলোকে সংযুক্ত করবে, যা আপনাকে খুবই কার্যকর একটি first-load অভিজ্ঞতা দিবে।

> বিঃদ্রঃ
>
> শুধুমাত্র সার্ভারে ব্যবহারযোগ্য। এই APIটি ব্রাউজারে পাওয়া যাবে না।
>
> এই মেথড যেই streamটি রিটার্ন করে সেটি আসলে একটি byte stream যা utf-8 এ এনকোড করা। আপনি যদি ভিন্ন কোন এনকোডের stream চান, তবে [iconv-lite](https://www.npmjs.com/package/iconv-lite) এর মত প্রজেক্ট দেখতে পারেন, যা streamকে ট্রান্সকোডিং টেক্সটের জন্য পরিবর্তন করতে পারে।

* * *

### `renderToStaticNodeStream()` {#rendertostaticnodestream}

```javascript
ReactDOMServer.renderToStaticNodeStream(element)
```

এটি [`renderToNodeStream`](#rendertonodestream) এর মতই, তবে React তার নিজ প্রয়োজনে যেসব অতিরিক্ত DOM attributes ব্যবহার করে, এটি তা তৈরি করে না, যেমন `data-reactroot`। যদি আপনি Reactকে static পেজ জেনারেটরের মত ব্যবহার করতে চান, তবে এটি খুব উপকারি, কারণ এটি অতিরিক্ত attributes মুছে ফেলে কিছু bytes বাঁচিয়ে দেয়।

এই stream থেকে আউটপুট পাওয়া HTML আসলে [`ReactDOMServer.renderToStaticMarkup`](#rendertostaticmarkup) যা রিটার্ন করে তার অনুরূপ।

আপনি যদি মার্কআপকে interactive করার জন্য ক্লায়েন্টে React ব্যবহার করার পরিকল্পনা করেন, তবে এই মেথডটি ব্যবহার করবেন না। এর পরিবর্তে সার্ভারে [`renderToNodeStream`](#rendertonodestream) এবং ক্লায়েন্টে [`ReactDOM.hydrate()`](/docs/react-dom.html#hydrate) ব্যবহার করুন।

> বিঃদ্রঃ
>
> শুধুমাত্র সার্ভারে ব্যবহারযোগ্য। এই APIটি ব্রাউজারে পাওয়া যাবে না।
>
> এই মেথড যেই streamটি রিটার্ন করে সেটি আসলে একটি byte stream যা utf-8 এ এনকোড করা। আপনি যদি ভিন্ন কোন এনকোডের stream চান, তবে [iconv-lite](https://www.npmjs.com/package/iconv-lite) এর মত প্রজেক্ট দেখতে পারেন, যা streamকে ট্রান্সকোডিং টেক্সটের জন্য পরিবর্তন করতে পারে।
