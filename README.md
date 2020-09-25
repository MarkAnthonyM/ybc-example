# ybc Bulma docs example
This repo contains a quick run down of some functionality of the ybc crate library. The examples here try to use the various examples listed in the bulma documention as a template, following closely in order to arrive at the same end result in both look and style. Hopefully this allows for some sort of synergy between both docs. For now only Navbar functionality is explored. If deemed beneficial, the rest of the ybc library will be explored, resulting a simple website that hopefully may act as a useful reference.

## Example - Navbar

<p align="center">
    <img src="https://i.imgur.com/sALk87F.gif">
</p>

### Code
<details>
    <summary>Example</summary>

```rust
struct Model {
    // `ComponentLink` is like a reference to a component.
    // It can be used to send messages to the component
    link: ComponentLink<Self>,
    value: i64,
}

impl Component for Model {
    type Message = Msg;
    type Properties = ();

    fn create(_props: Self::Properties, link: ComponentLink<Self>) -> Self {
        Self {
            link,
            value: 0,
        }
    }

    fn update(&mut self, msg: Self::Message) -> ShouldRender {
        match msg {
            Msg::AddOne => {
                self.value += 1;
                // the value has changed so we need to
                // re-render for it to appear on the page
                true
            }
        }
    }

    fn change(&mut self, _props: Self::Properties) -> ShouldRender {
        // Should only return "true" if new properties are different to
        // previously received properties.
        // This component has no properties so we will always return "false".
        false
    }

    fn view(&self) -> Html {
        html! {
            // ybc-element of type Navbar. navbrand, navstart, and navend properties are required and each expect an Html type. The yew html! macro returns this type.
            // The `navbar-burger` section is automatically appended.
            <ybc::Navbar navbrand=self.view_navbrand() navstart=self.view_navstart() navend=self.view_navend() />
        }
    }
}

impl Model {
    // Contruct the contents of the Navbar brand section and return Html type that navbrand property of Navbar expects.
    // Html type gets tossed into navbrand field of NavbarProps struct. Consult ybc Docs for more info.
    fn view_navbrand(&self) -> Html {
        html! {
            <>
                <ybc::NavbarItem tag=A>
                    <img src="https://bulma.io/images/bulma-logo.png" />
                </ybc::NavbarItem>
            </>
        }
    }

    // Contruct Navbar navdrop Menu
    fn view_navdrop(&self) -> Html {
        html! {
            <ybc::NavbarDropdown navlink=self.view_navlink() hoverable=true>
                <ybc::NavbarItem tag=A>
                    { "About" }
                </ybc::NavbarItem>
                <ybc::NavbarItem tag=A>
                    { "Jobs" }
                </ybc::NavbarItem>
                <ybc::NavbarItem tag=A>
                    { "Contact" }
                </ybc::NavbarItem>
                <ybc::NavbarDivider />
                <ybc::NavbarItem tag=A>
                    { "Report an issue" }
                </ybc::NavbarItem>
            </ybc::NavbarDropdown>
        }
    }

    // Contruct the contents of the `navbar-end` section and return Html type that navend property of Navbar expects.
    // Html type gets tossed into navend field of NavbarProps struct. Consult ybc Docs for more info.
    fn view_navend(&self) -> Html {
        html! {
            <ybc::NavbarItem tag=Div>
                // Create div container for button groups
                <ybc::Buttons>
                    // Button classes property accepts Option<String> type. `is-primary` here provides color styling. 
                    <ybc::Button classes=Some("is-primary")>
                        <strong>{ "Sign up" }</strong>
                    </ybc::Button>
                    <ybc::Button classes=Some("is-light")>
                        { "Log in" }
                    </ybc::Button>
                </ybc::Buttons>
            </ybc::NavbarItem>
        }
    }

    // Contruct the contents of the `navbar-link` section and return Html type that navlink property of NavbarDropdown expects.
    // Html type gets tossed into navlink field of NavbarDropdownProps struct. Consult ybc Docs for more info.
    fn view_navlink(&self) -> Html {
        html! {
            { "More" }
        }
    }

    // Contruct the contents of the `navbar-start` section and return Html type that navstart property of Navbar expects.
    // Html type gets tossed into navstart field of NavbarProps struct. Consult ybc Docs for more info.
    fn view_navstart(&self) -> Html {
        html! {
            <>
                <ybc::NavbarItem tag=A>
                    { "Home" }
                </ybc::NavbarItem>
                <ybc::NavbarItem tag=A>
                    { "Documentation" }
                </ybc::NavbarItem>
                { self.view_navdrop() }
            </>
        }
    }
}

fn main() {
    yew::start_app::<Model>();
}
```
    
</details>