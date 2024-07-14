---
title: "BlockNote의 Text-To-Speech 기능 구현 방법 - Nextjs 사용 "
description: ""
coverImage: "/TIL/assets/img/2024-07-14-ImplementingText-To-SpeechFunctionalityForBlockNoteInNextJs_0.png"
date: 2024-07-14 20:49
ogImage: 
  url: /TIL/assets/img/2024-07-14-ImplementingText-To-SpeechFunctionalityForBlockNoteInNextJs_0.png
tag: Tech
originalTitle: "Implementing Text-To-Speech Functionality For BlockNote In NextJs"
link: "https://medium.com/@programming-advice/implementing-text-to-speech-functionality-for-blocknote-in-next-js-72c6b79be739"
---


## 와우: WYSIWYG 편집기로 텍스트 음성 변환 기능!

로켓 과학을 해결했습니다! 물론 거짓말이에요. BlockNote에 텍스트 음성 변환 기능을 Next.Js에서 어떻게 구현할지 알아냈습니다. BlockNote를 모르시면, Next.js에서 WYSIWYG 편집기 문제에 대한 해결책을 발견한 기사를 읽어보세요.

그러니까 읽어보세요. 하지만 여러분의 지식 부족함에도 불구하고, 저는 계속 이 기사를 쓸 겁니다. BlockNote를 아는 여러분은 이 기사로 들어가 봅시다. 출발!

![이미지](/TIL/assets/img/2024-07-14-ImplementingText-To-SpeechFunctionalityForBlockNoteInNextJs_0.png)

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 요약

우리는 블록을 HTML로 변환하여 Firebase에 BlockNote 데이터를 저장하는 방법을 배웠어요. 그 기사를 읽었다면 가상으로 엄지척을 할게요. 그럼, 코드는 지금 이렇게 보일 거예요:

```js
'use client';
import React, { useState, useEffect, useRef, ChangeEvent, useCallback } from 'react';
import { useSearchParams } from 'next/navigation';
import { firestore } from '../../../../../firebase';
import { getDoc, doc, updateDoc } from 'firebase/firestore';
import "@blocknote/core/fonts/inter.css";
import { useCreateBlockNote } from "@blocknote/react";
import { BlockNoteView } from "@blocknote/mantine";
import "@blocknote/mantine/style.css";
import { Block } from "@blocknote/core";

function Document() {
 const params = useSearchParams();
 const docId = params.get("id");
 const [title, setTitle] = useState('');
 const [value, setValue] = useState('');
 const [blocks, setBlocks] = useState<Block[]>([]);

   useEffect(() => {
   const fetchDocument = async () => {
     if (!docId) return;

     const docRef = doc(firestore, `documents/${docId}`);
     try {
       const docSnap = await getDoc(docRef);
       if (docSnap.exists()) {
         const data = docSnap.data();
         setTitle(data.title || '');
         setValue(data.content || '');
       } else {
         console.log('Document does not exist');
       }
     } catch (error) {
       console.error('Error fetching document: ', error);
     }
   };

   fetchDocument();
 }, [docId]);

 const editor = useCreateBlockNote();

 useEffect(() => {
   async function loadInitialHTML() {
     const blocks = await editor.tryParseHTMLToBlocks(value);
     editor.replaceBlocks(editor.document, blocks);
   }
   loadInitialHTML();
 }, [editor, value]);

 const saveDocument = async () => {
   if (!docId) return;

   const content = await editor.blocksToHTMLLossy(blocks);

   try {
     await updateDoc(doc(firestore, `documents/${docId}`), {
       content: content,
     });

     console.log('Document saved successfully');
   } catch (error) {
     console.error('Error saving document: ', error);
   }
 };

 return (
   <div>
     <h1>{title}</h1>
     <BlockNoteView editor={editor} onChange={() => { setBlocks(editor.document); }} />
     <button onClick={saveDocument}>Save Document</button>
   </div>
 );
}

export default Document;
```

우리가 머릿속을 새롭게 한 이제, 뭔가 추가해봐요!

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 이것을 어떻게 할 건가요???

좋은 질문이네요! 제가 떠올렸기 때문에 알고 있어요. 웹 스피치 API를 사용하여 텍스트 음성 변환 기능을 구현할 거에요.

웹 스피치 API를 활용하면 음성 데이터를 웹 앱에 통합할 수 있어요. 이 경우는 Next.js 애플리케이션에서 사용할 거에요.

이러한 종류의 기능을 추가하기 위해 SpeechSynthesis라고 불리는 Web 스피치 API의 인터페이스를 사용할 거에요.

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 구현

우리의 코드에는 단지 한 가지 함수와 버튼을 추가하면 됩니다. 그게 다에요. 코드에 다음 함수를 추가하세요:

```js
function speak() {
  const text = blocks
  .map(block => block?.content)
  .filter(content => content !== undefined)
  .flatMap(content => {
    if (Array.isArray(content)) {
      return content
        .filter(contentItem => contentItem.type === 'text')
        .map(contentItem => contentItem.text);
    } else {
      return [];
    }
  })
  .join(' ');
  let utterance = new SpeechSynthesisUtterance(text);
  let voicesArray = speechSynthesis.getVoices();
  utterance.voice = voicesArray[2];
  speechSynthesis.speak(utterance);
}
```

이 함수는 문서에서 콘텐츠를 가져와서 콘텐츠가 텍스트 유형인 경우 에디터 내용을 말합니다. 간단하죠. 이제 '말하기' 버튼을 화면에 표시하세요:

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

```jsx
<button onClick={speak}>Speak</button>
```

# 더 깊이 파고들기

만약 음성 합성의 피치, 속도, 볼륨을 변경하여 사용자 정의하고 싶다면 어떻게 할까요? 잘하셨어요! 이제 이에 대해 알아볼게요. 이 뒤에 광고가 나와야죠! 농담이죠: 여기는 유튜브가 아니에요.

피치 변경하기

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

음 높이를 조절하려면 이 줄을 추가하세요:

```js
utterance.pitch = 4; // 원하는 값으로 숫자를 변경하세요
```

속도 변경하기

속도를 조절하려면 이 줄을 추가하세요:

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
utterance.rate= 5; // 원하는 값으로 숫자를 변경해주세요
```

볼륨 조절하기

```js
utterance.volume= 0.4; // 원하는 값으로 소수를 변경해주세요
```

지금까지 코드에 텍스트 음성 변환 기능을 구현하는 단 기사를 마무리 지어보았습니다. 만약 이 기사가 도움이 되었다면 박수를 보내주세요, 팔로우를 눌러주세요, 또는 응답을 달아주세요.

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

행복한 코딩하세요!