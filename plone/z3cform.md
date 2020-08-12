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
