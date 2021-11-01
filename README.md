# MSG Reader TS

MSG Reader is an Outlook Item File (.msg) reader that is built with HTML5. 

Allows to parse and extract necessary information (attachment included) from .msg file.

Based on original TypeScript conversion from [here](https://www.npmjs.com/package/as-msg-reader)


## Online demo

+ http://ykarpovich.github.io/msg.reader/examples/example.html

## Angular service example

```typescript
import { Injectable } from '@angular/core';
import * as MsgReader from '@sharpenednoodles/msg.reader-ts';
import { MSGFileData } from '@sharpenednoodles/msg.reader-ts';
import { Observable, of } from 'rxjs';

export class MsgParserService {
  constructor() { }

  parseMsg(msgFile: File): Observable<MSGFileData> {
    const fileReader = new FileReader();

    fileReader.readAsArrayBuffer(msgFile);

    return new Observable((observer) => {
      fileReader.onload = (e: ProgressEvent): void => {
        let bytes = new Uint8Array((<any>e.target).result);

        const msgReader = new MsgReader.MSGReader(bytes);
        const msg = msgReader.getFileData();

        if (msg['error']) { observer.error(msg['error']); }

        observer.next(msg as MSGFileData);
        observer.complete();
      }

      fileReader.onerror = (error: ProgressEvent): void => {
        observer.error(error);
      }
    });
  }
}
```

## Fork
This project was orignally forked from:

https://github.com/ykarpovich/msg.reader
