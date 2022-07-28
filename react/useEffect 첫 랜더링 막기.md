# useEffect 첫 랜더링 막기

1. `useDidMountEffect.tsx` 생성하기

   ```react
   import  { useRef, useEffect } from "react";
   
   export const useDidMountEffect = (func: any, deps: any) => {
     const didMount = useRef(false);
   
     useEffect(() => {
       if (didMount.current) func();
       else didMount.current = true;
     }, deps);
   };
   ```

2. 내가 만든 함수에 적용하기

   ```react
   const testValue = useRecoilValue<string>(testvalue);
   
   const test = () => {
         const checkboxlist : NodeListOf<Element>  = document.getElementsByName('checkbox');
         const project = document.getElementsByClassName("project");
         const checkbox_yellow = document.getElementsByClassName('checkbox-all');
         for (let  item of checkboxlist as any) {
           item.checked = false;
           const eClassName = item.getAttribute('class')?.split('-')[1]
         }
       }
   ```

   ```react
   useDidMountEffect ((): void => {
         test();
   }, [testValue]);
   ```

   