# Server Side Events ref: ChatGPT


```py
from fastapi import FastAPI
from fastapi.responses import StreamingResponse
import asyncio
import json

app = FastAPI()

async def event_generator():
    """Simulates a live event stream"""
    count = 0
    while True:
        count += 1
        yield f"data: {json.dumps({'message': f'Update {count}'})}\n\n"
        await asyncio.sleep(2)  # Simulate new data every 2 seconds

@app.get("/events")
async def sse():
    return StreamingResponse(event_generator(), media_type="text/event-stream")

```


Adapt to FastHTML ref: self
---
```py
from starlette.responses import StreamingResponse
from datetime import datetime

@rt
def index():
    markup = (
        Div(id="event-stream", 
            hx_ext="sse", 
            sse_connect="/events", 
            sse_swap="message")
        
    )
    return markup


async def event_generator():
    """Simulates a live event stream"""
    count = 0
    while True:
        count += 1
        now = datetime.now().time()
        data = {
            'message': f'Update #{count} {now:%H:%M:%S.%f}'
        }
        
        yield f"data: {json.dumps(data)}\n\n"
        await asyncio.sleep(.1)  


@app.get("/events")
async def sse():
    return StreamingResponse(event_generator(), media_type="text/event-stream")

```

outputs about 10 messages with timestamp with micros
`{"message": "Update #4862 14:35:35.756070"}`. The time taken to loop can be seen as drift in the micros.

Opening multiple tabs creates a new generator. 




# Inserting components 

* [ref fabage and jeremy](https://github.com/fabge/fasthtml-sse/blob/main/main.py) 
* [ref Chris Levy](https://drchrislevy--drchrislevy-serve.modal.run/blog/blog_post?fpath=posts%2Fsse%2Fsse.ipynb)

Simplified example:  The only difference here is the wrapper `sse_message`

```python
@rt
def index():
    markup = (
        Div(id="event-stream", 
            hx_ext="sse", 
            sse_connect="/events", 
            sse_swap="message")
        
    )
    return markup


async def event_generator():
    """Simulates a live event stream"""
    count = 0
    while True:
        count += 1
        now = datetime.now().time()

        yield sse_message(Button(f'Update #{count} {now:%H:%M:%S.%f}'), event='message')
        await asyncio.sleep(1) 


@app.get("/events")
async def sse():
    return StreamingResponse(event_generator(), media_type="text/event-stream")
```