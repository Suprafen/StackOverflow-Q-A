## How to apply a custom button style to a toolbar menu?
### Source:
[Source link](https://stackoverflow.com/questions/78072547/how-to-apply-a-custom-button-style-to-a-toolbar-menu/78072729#78072729)

## Answer

You were almost there, but instead of conforming to `ButtonStyle`, you would need to conform to `MenuStyle` to create your own custom style for menu. Though, keep in mind, that it won't work with systemImage like in your case. The way you can provide system image, is by passing needed parameter to the style's initializer.


    struct CustomMenuToolbarStyle: View {
        var body: some View {
            NavigationStack {
                VStack {
                    Text("Other content")
                }
                .toolbar {
                    Menu("Filter") {
                        Button("Action 1") {}
                        Button("Action 2") {}
                        Button("Action 3") {}
                    }
                    .menuStyle(CustomMenuStyle(systemImage: "line.3.horizontal.decrease.circle"))
                }
            }
        }
    }
    
    struct CustomMenuStyle: MenuStyle {
        let systemImage: String
        
        func makeBody(configuration: Configuration) -> some View {
            HStack {
                Menu(configuration)
                Image(systemName: systemImage)
            }
            .padding(8)
            .foregroundColor(.white)
            .background(.blue, in: RoundedRectangle(cornerRadius: 10))
        }
    }

**Result is:** 

[![Screenshot of the result][1]][1]

  [1]: https://i.stack.imgur.com/pBVfOl.png?
