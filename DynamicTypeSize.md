## How to apply a custom button style to a toolbar menu?
### Source:
[Source link](https://stackoverflow.com/questions/78257388/dynamictypesize-not-working-on-the-sheet/78259698#78259698)

## Answer

I would add a modifier, which checks against current dynamicTypeSize and the maximum desired type size.

1. Create view modifier

```
struct DynamicTypeModifier: ViewModifier {
    
    var currentTypeSize: DynamicTypeSize
    
    init(currentTypeSize: DynamicTypeSize, maxTypeSize: DynamicTypeSize) {
        switch currentTypeSize {
        case maxTypeSize...(.accessibility5):
            self.currentTypeSize = maxTypeSize
        default:
            self.currentTypeSize = currentTypeSize
        }
    }
    
    func body(content: Content) -> some View {
        content.dynamicTypeSize(currentTypeSize)
    }
}
```

2. Define an extension for View with newly created modifier:

```
extension View {
    func restrictDynamicType(to restrictedTypeSize: DynamicTypeSize,
                             currentTypeSize: DynamicTypeSize) -> some View{
        self.modifier(DynamicTypeModifier(currentTypeSize: currentTypeSize,
                                          maxTypeSize: restrictedTypeSize))
    }
}
```

Usage:
It is very important to pass environment dynamic type size value to the view  which you want to be restricted by provided max dynamic type size, to have current type size. Otherwise, you can move applied modifier right to the place where you use the view, instead of appliance inside the body.

Here're two examples, choose whichever you like:

Parent view holds environment `.dynamicTypeSize` and pass it to the sheet view, because sheet view has its own environment `.dynamicTypeSize`. Our modifier is applied inside of sheet view body.

```
struct ContentView: View {
    @State var isPresented: Bool = false
    @Environment(\.dynamicTypeSize) var dynamicTypeSize
    var body: some View {
        VStack {
            Button("Open sheet") {
                isPresented.toggle()
            }
        }
        .sheet(isPresented: $isPresented) {
            SheetView()
                .environment(\.dynamicTypeSize, dynamicTypeSize)
        }
    }
}
```
```
struct SheetView: View {
    @Environment(\.dynamicTypeSize) var dynamicTypeSize
    var body: some View {
        VStack {
            Text("Text sample")
                .restrictDynamicType(to: .xxxLarge, currentTypeSize: dynamicTypeSize)
        }
    }
}
```
Parent view holds environment `.dynamicTypeSize` and pass it to our modifier, which is applied to a view right where we're using it.

```
struct ContentView: View {
    @State var isPresented: Bool = false
    @Environment(\.dynamicTypeSize) var dynamicTypeSize
    var body: some View {
        VStack {
            Button("Open sheet") {
                isPresented.toggle()
            }
        }
        .sheet(isPresented: $isPresented) {
            SheetView()
                .restrictDynamicType(to: .xxxLarge, currentTypeSize: dynamicTypeSize)
        }
    }
}
```
```
struct SheetView: View {
    var body: some View {
        VStack {
            Text("Text sample")
        }
    }
}
```
