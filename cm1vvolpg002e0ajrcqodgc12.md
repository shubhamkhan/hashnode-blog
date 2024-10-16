---
title: "Debouncing vs Throttling: Techniques for Efficient JavaScript Function Calls"
seoTitle: "Debouncing vs Throttling: Techniques for Efficient JavaScript Function"
seoDescription: "Two effective techniques, debouncing and throttling. These methods help regulate how often a function is executed under frequent event event triggers."
datePublished: Sat Oct 05 2024 08:14:19 GMT+0000 (Coordinated Universal Time)
cuid: cm1vvolpg002e0ajrcqodgc12
slug: debounce-vs-throttle
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729102283540/2fed1ce8-ae84-48e5-9e9d-f4d8f0e70aca.webp
tags: optimization, javascript, apis, reactjs, javascript-framework, debouncing, throttling, function-calling, debouncing-and-throttling

---

Performance optimisation is crucial when developing modern web applications, especially those with interactive elements. Events like typing in a search bar, scrolling, or resizing a window can trigger many function calls in a very short period, potentially overloading the system and degrading the user experience. Two effective techniques to address this are **debouncing** and **throttling**. These methods help regulate how often a function is executed under frequent event triggers, thus improving the efficiency of your application.

In this article, we'll cover both debouncing and throttling, along with their use cases, examples, and how to implement them as custom React hooks for better reuse across components.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728115566006/4cc540cb-6473-4eea-b40f-9cae1114e362.gif align="center")

### **Debouncing: Delaying Action Until the User is Done**

Debouncing is a technique that postpones the execution of a function until the user has stopped triggering an event for a specified amount of time. This is especially useful in scenarios like search inputs, where you want to delay sending API requests until the user has finished typing, avoiding an API call with every keypress.

#### **How Debouncing Works**

Imagine you're building a search bar that fetches results from an API. Instead of sending a request for each character typed, debouncing allows you to send a request only when the user has stopped typing for a few milliseconds, say 300ms. If the user types again within this time frame, the timer resets, and no API call is made.

#### **Debouncing Example**

```javascript
function debounce(func, wait) {
  let timeout;
  return function() {
    const context = this;
    const args = arguments;
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      func.apply(context, args);
    }, wait);
  };
}

function searchAPI() {
  console.log("API call made");
}

const debouncedSearch = debounce(searchAPI, 300);

// The API call will only be made 300ms after the user stops typing.
debouncedSearch();
```

Here, the `debounce` the function takes two parameters: the function to be debounced (`func`) and the wait time (`wait`) in milliseconds. The function `searchAPI` will only be executed once the user has stopped typing for 300ms. If the user types again before the 300ms have passed, the previous call is cancelled, and the timer restarts.

### **Throttling: Limiting the Frequency of Function Calls**

Throttling, unlike debouncing, ensures that a function is called at most once in a specified time period, regardless of how many times an event is triggered. This is particularly useful in scenarios like window resizing or scrolling, where the event fires repeatedly in a very short period.

#### **How Throttling Works**

When you throttle a function, you set a limit on how often the function can be executed. Even if the event (e.g., scrolling) triggers the function repeatedly, the function will only execute once in the defined time window (e.g., once every 200 milliseconds). Any additional triggers within that time window will be ignored.

#### **Throttling Example**

```javascript
const throttle = (func, limit) => {
  let timer = 0;
  return function() {
    const now = new Date().getTime();
    if (now - timer>= limit) {
      timer = now;
      func.apply(this, arguments);
    }
  };
};

const updateLayout = () => {
  console.log("Layout updated");
};

const throttledUpdate = throttle(updateLayout, 200);

// updateLayout will only be called once every 200ms
window.addEventListener("resize", throttledUpdate);
```

In this example, the `throttle` the function ensures that the `updateLayout` function can only be executed once every 200 milliseconds, even if the window resize event fires more frequently.

### **Using Debounce and Throttle in React: Custom Hooks**

In React applications, it's common to implement debouncing or throttling for various user interactions like form inputs, window events, or even button clicks. To make this process more reusable, we can encapsulate the logic for both debouncing and throttling inside custom hooks.

#### **Custom Hook for Debouncing**

Below is a React custom hook `useDebounce` that accepts a function and a delay. It returns a debounced version of the function, which you can use in your components.

```javascript
import { useRef, useCallback } from "react";

const useDebounce = (func, delay) => {
  const timer = useRef(null);

  return useCallback(
    (...args) => {
      return new Promise((resolve, reject) => {
        if (timer.current) {
          clearTimeout(timer.current);
        }
        timer.current = setTimeout(() => {
          func(...args)
            .then(resolve)
            .catch(reject);
        }, delay);
      });
    },
    [func, delay]
  );
};

export default useDebounce;
```

#### **Using** `useDebounce` Hook

```javascript
import React, { useState } from "react";
import useDebounce from "./useDebounce";

const SearchComponent = () => {
  const [searchTerm, setSearchTerm] = useState("");

  const fetchResults = async (query) => {
    console.log(`Fetching results for ${query}`);
    // Simulate an API call
    return new Promise((resolve) => setTimeout(resolve, 1000));
  };

  const debouncedFetchResults = useDebounce(fetchResults, 300);

  const handleSearch = async (e) => {
    setSearchTerm(e.target.value);
    await debouncedFetchResults(e.target.value);
  };

  return (
    <input
      type="text"
      value={searchTerm}
      onChange={handleSearch}
      placeholder="Search..."
    />
  );
};

export default SearchComponent;
```

Here, `useDebounce` ensures that the `fetchResults` function only gets called 300ms after the user has stopped typing.

#### **Custom Hook for Throttling**

Similarly, you can create a custom `useThrottle` hook for throttling. This hook ensures that a function is called at most once within a given interval, even if it's triggered multiple times during that period.

```javascript
import { useRef, useCallback } from "react";

const useThrottle = (func, limit) => {
  const timer = useRef(0);

  return useCallback(
    (...args) => {
      const now = new Date().getTime();
      if (now - timer.current >= limit) {
        timer.current = now;
        func(...args);
      }
    },
    [func, limit]
  );
};

export default useThrottle;
```

#### **Using** `useThrottle` Hook

```javascript
import React from "react";
import useThrottle from "./useThrottle";

const ScrollComponent = () => {
  const handleScroll = () => {
    console.log("Scrolled!");
  };

  const throttledScroll = useThrottle(handleScroll, 500);

  React.useEffect(() => {
    window.addEventListener("scroll", throttledScroll);

    return () => {
      window.removeEventListener("scroll", throttledScroll);
    };
  }, [throttledScroll]);

  return <div style={{ height: "200vh" }}>Scroll down to see the effect</div>;
};

export default ScrollComponent;
```

In this example, the `handleScroll` function will be executed at most once every 500 milliseconds, no matter how frequently the scroll event occurs.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728115618454/b0939597-fe87-4373-b466-fff8ada79c2b.gif align="center")

### Conclusion

Both **debouncing** and **throttling** are essential techniques for improving performance in web applications by controlling the rate of function executions. Debouncing is particularly useful for handling user input events like search boxes, while throttling works best for events triggered frequently, such as scrolling or resizing. By utilizing these techniques within custom hooks like `useDebounce` and `useThrottle`, you can optimize performance in your React applications, making them more efficient and responsive.