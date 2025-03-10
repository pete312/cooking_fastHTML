
# context menu 

ref self.  First go at this.

```python
@rt
def index(val:str=''):
    MENU='block px-4 py-2 hover:bg-gray-100'
    markup = (
        Div(f"menu {val}", 
            Div(
                A("open",  hx_get=select_option.to(val="open"),  hx_swap='innerHTML', hx_target='#my-target', cls=MENU),
                A("close", hx_get=select_option.to(val="close"), hx_swap='innerHTML', hx_target='#my-target', cls=MENU),
                A("edit",  hx_get=select_option.to(val="edit"),  hx_swap='innerHTML', hx_target='#my-target', cls=MENU),
                cls='absolute hidden bg-white shadow-lg border rounded py-2 z-10',
                id='context-menu'
            ),
            id="context-area", 
            cls="p-10 bg-gray-200 cursor-pointer",
            hx_get="#",
            hx_swap='none',
            hx_trigger='contextmenu',
            hx_on__before_request="htmx.find('#context-menu').classList.toggle('hidden')",
        ),
        Card(Div(f"body text {val}", id='my-target'))
    ), Script("document.addEventListener('contextmenu', function(event) { event.preventDefault(); });")
    return markup

@rt
def select_option(val:str):
    return Div(f"action with {val}")
```