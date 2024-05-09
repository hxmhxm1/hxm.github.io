---
title: nextjs遇到的问题
---

## Hydration(水合)错误

- 是什么： 在渲染你的应用程序时，预渲染的react树（ssr或ssg) 和在浏览器中第一次渲染时渲染的react树之间存在差异。这一次渲染被称为hydration,这是react的一个特性

- 不规范的html标签

  ```html
  <P>
    <div></div>
  </p>
  ```

- 数据问题，数据在服务端和首次客户端渲染展示的数据不一致
  例如我在代码中使用了import { isMobile } from 'react-device-detect';
  但是因为isMobile默认是false,但是如果在移动端设置中，首次渲染时是true，这就造成了hydration问题
  解决方法就是在useEffect中执行判断，使得判断的变量始终在浏览器端才执行，下面代码是我自己为了解决这个isMobile问题封装的简单钩子

  ```js
  import { useEffect, useState } from 'react';
  import { isMobile } from 'react-device-detect';

  const useIsMobile = () => {
    const [mobile, setMobile] = useState(true);
    useEffect(() => {
      setMobile(isMobile);
    }, [isMobile]);
    return mobile;
  };

  export default useIsMobile;
  ```
