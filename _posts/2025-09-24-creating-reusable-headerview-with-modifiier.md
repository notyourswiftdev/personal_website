---
layout: post
title:  "Creating reusable HeaderView with ViewModifier in SwiftUI"
---

# Creating reusable HeaderView with ViewModifier in SwiftUI

ViewModifiers "A modifier that you apply to a view or another view modifier, producing a different version of the original value." What does that mean?! Let me show you.

**View**
```
struct HeaderView: View {
    var onProfileTap: () -> Void = {}
    
    var body: some View {
        HStack(spacing: 12) {
            HStack(spacing: 8) {
                Image(systemName: "house.fill")
                    .imageScale(.large)
                Text("Title Header")
                    .font(.headline)
                    .fontWeight(.semibold)
            }
            Spacer()
            Button {
                onProfileTap()
            } label: {
                Image(systemName: "person.circle.fill")
                    .imageScale(.large)
            }
            .buttonStyle(.plain)
        }
        .padding(.horizontal, 16)
        .frame(height: 56)
        .background(.ultraThinMaterial)
        .overlay(alignment: .bottom) {
            Divider()
        }
    }
}
```
**ViewModifier**
```
struct HeaderViewModifier<Header: View>: ViewModifier {
    @ViewBuilder var header: () -> Header
    
    func body(content: Content) -> some View {
        content
            .safeAreaInset(edge: .top) {
                header()
            }
    }
}
```
**ViewModel**
```
final class HeaderViewViewModel: ObservableObject {
    
}

```

**Extension**
```
func appHeader() -> some View {
        modifier(HeaderViewModifier {
            HeaderView()
        })
    }
    
func appHeader<Header: View>(@ViewBuilder _ header: @escaping () -> Header) -> some View {
    modifier(HeaderViewModifier(header: header))
}
```