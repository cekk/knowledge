# ZCA hints

## Customize view without touching the original template

```xml
    <configure package="original.package.browser">
        <browser:page
            for=".something.ISomething"
            name="my_view"
            class="custom.package.browser.view.MyView"
            template="my_view.pt"
            layer="custom.package.interfaces.ICustomPackageLayer"
            permission="zope2.View"
        />
    </configure>
```

with zcml `<configure>` directive you can override a browserview without touching its template
(for example if you want to override only the class or the permission).

for `for` and `class` attributes you nedd to provide the full dotted name of classes of custom package, and let
the template path as is (Plone will search it into the original package).
