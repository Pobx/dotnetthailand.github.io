---
title: React useState & useEffect
showMetadata: true
editable: true
showToc: true
---

สวัสดีฮะทุกคน วันนี้เราจะมาเรียนรู้เรื่อง Basic Hooks กันฮะ ซึ่ง Hook ตัวแรกที่เราจะมาเรียนรู้กันก็คือ State Hook กันฮะ

### Hook คืออะไร ?
Hook คือ Feature ใหม่ที่เข้าใน React version 16.8 ที่ทำให้เราสามารถจัดการ State ที่ Function Component ได้โดยที่ไม่ต้องเขียน Class Component

### กฏการใช้ Hooks
- เรียกใช้งาน Hook ที่ React function components
- เรียกใช้งาน Hook ที่ Top Level React function component

หมายเหตุ: อย่าเรียกใช้งาน Hooks ใน Regular javascript function.

### State Hook
State Hook คือการ Update Current State ให้กับ React ได้รู้และทำการ re-render ที่ UI ตามตัวอย่างด้านล่าง
```jsx
import React, { useState } from 'react';
import ReactDOM from 'react-dom';

function MyUseState() {
	const [count, setCount] = useState(0);
	const updateCount = () => setCount(prevValue => prevValue + 1);
	return <>
	Count is : {count}   <button type="button" onClick={updateCount}>Add Count</button>
	</>;
}

ReactDOM.render(
	<React.StrictMode>
		<MyUseState />
	</React.StrictMode>,
	document.getElementById('root')
);
```

สำหรับตัวอย่างด้านบนนี้เป็นการใช้งาน useState ในการ initial ค่าให้กับ state เท่ากับ 0 ในครั้งแรกที่ Render function component
จากนั้นเมื่อมีการกดที่ button event onClick จะไปเรียก updateCount function ภายใน updateCount ก็จะไปเรียกใช้งาน
setCount เพื่อทำการเพิ่มค่าเข้าไปอีก 1 จาก current state

หมายเหตุ: 
 - เราไม่สามารถ update current state ของ count ได้โดยตรงต้องทำผ่าน setCount function เท่านั้น
 - useState จะ return ค่าออกมาเป็น array 2 อย่างคือ current state(count) และ setState(setCount) เสมอทั้งนี้สำหรับชื่อ count และ setCount เราสามารถตั้งเชื่อป็นอะไรก็ได้
 - useState อยู่ภายใต้ condition ไม่ได้

### Effect Hook
Effect Hook คือการทำงานที่มีผลกระทบจากปัจจัยภายนอกซึ่งลักษณะการทำงานจะเหมือนกับ componentDidMount และ componentDidUpdate ที่มีอยู่ใน Class Component ตามตัวอย่างด้านล่าง
```jsx
import React, { useEffect, useState } from 'react';
import ReactDOM from 'react-dom';

function UseEffectFetchData() {
	const [resourceType, setResourceType] = useState('todos');
	const [content, setContent] = useState([]);
	const [albums, setAlbums] = useState([]);

	useEffect(() => {
		fetch(`https://jsonplaceholder.typicode.com/albums`)
			.then((response) => response.json())
			.then((json) => setAlbums(json));
	}, []);

	useEffect(() => {
			fetch(`https://jsonplaceholder.typicode.com/${resourceType}`)
			.then((response) => response.json())
			.then((json) => setContent(json));
	}, [resourceType]);

	return (
		<>
			<button type="button" onClick={() => setResourceType('todos')}>
				Fetch Todos
			</button>
			<button type="button" onClick={() => setResourceType('users')}>
				Fetch Users
			</button>
			<hr />
			albums
			{JSON.stringify(albums)}
			<hr />
			<p>{resourceType}</p>
			{content.map((item, key) => (
				<pre key={key}>{JSON.stringify(item)}</pre>
			))}
		</>
	);
}

ReactDOM.render(
	<React.StrictMode>
		<UseEffectFetchData />
	</React.StrictMode>,
	document.getElementById('root')
);

```

สำหรับตัวอย่างด้านบนนี้เป็นการใช้งาน useState และ useEffect ร่วมกัน ซึ่งในตัวอย่างนี้มีทั้งการใช้งาน useEffect แบบที่มีปัจจัยภายนอกแบบคงที่([]) และปัจจัยภายนอกแบบแปรผัน([resourceType])
ซึ่งในกรณีที่มีปัจจัยภายนอกแบบแปรผัน กรณีที่ current state ของ resourceType มีการเปลี่ยนแปลง function ที่อยู่ภายใน useEffect จะถูกเรียกใช้งานทันที(componentDidUpdate) แต่หาก current state ไม่มีการเปลี่ยนแปลง useEffect ก็จะไม่เรียกการทำงานใดๆภายใน useEffect ในกรณีที่ใช้ [] จะหมายถึงการเรียกใช้งาน useEffect ครั้งเดียวตอนที่ Component โดนเรียกใช้งาน(componentDidMount)
เพราะว่า [] (empty array) ไม่มีการเปลี่ยนค่าจากปัจจัยภายนอกนั้นเอง

สุดท้ายนี้ก็หวังว่าจะเป็นประโยชน์กับทุกๆท่านที่เริ่มเขียน React นะฮะ ส่วนตัวผมนั้นขอตัวไปเรียนรู้ React ต่อก่อนละฮะ แล้วพบกันใหม่ :)