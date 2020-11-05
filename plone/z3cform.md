# z3c.form hints

## Add readonly field
https://community.plone.org/t/computed-field-for-dexterity/11405

If the text value is not set, use this:
    
```python
class MyForm(Form):
    def updateWidgets(self):
        super(MyForm, self).updateWidgets()

        self.widgets["field_id"].value = Markup(
            "<div>Congrats <b>1_000_000th</b> visitor!</div>"
        )
```

## Extend fields list on a form

First of all, we need to create the adapter::

```python
# -*- coding: utf-8 -*-
from some.package.browser.form import OriginalForm
from persistent import Persistent
from plone.z3cform.fieldsets import extensible
from some.package.content import OriginalContentType
from z3c.form.field import Fields
from zope import schema
from zope.annotation import factory
from zope.component import adapter
from zope.interface import implementer
from zope.interface import Interface
from zope.publisher.interfaces.browser import IDefaultBrowserLayer

# define a schema
class IMyExtenderSchema(Interface):
    new_field = schema.TextLine(title=u"New field", required=False)

# Persistent class that implements the IMyExtenderSchema interface and is adapted to a context
@adapter(OriginalContentType)
@implementer(IMyExtenderSchema)
class ExtenderFields(Persistent):
    new_field = u""

ExtenderFactory = factory(ExtenderFields)


# Extending the form with the fields defined in the
# IMyExtenderSchema interface.
@adapter(Interface, IDefaultBrowserLayer, OriginalForm)
class Extender(extensible.FormExtender):

    fields = Fields(IMyExtenderSchema)

    def __init__(self, context, request, form):
        self.context = context
        self.request = request
        self.form = form

    def update(self):
        self.add(IMyExtenderSchema, prefix="")
        # here you can also move or customize fields
```

And then you need to register these adapters::

```xml
<adapter
  factory=".my_extender.ExtenderFactory"
  provides=".my_extender.IExtenderFields" />

<adapter
  factory=".my_extender.Extender"
  provides="plone.z3cform.fieldsets.interfaces.IFormExtender" />
```

References:
- https://pythonhosted.org/plone.app.discussion/howtos/howto_extend_the_comment_form.html
- https://pypi.org/project/plone.z3cform/#fieldsets-and-form-extenders
