# Upgrade step utils

```python
def set_behavior(fti, name, value):
    # add or remove the behavior based on the value from the form
    behaviors = list(fti.behaviors)
    if value and name not in behaviors:
        behaviors.append(name)
    elif not value and name in behaviors:
        behaviors.remove(name)
    fti.behaviors = tuple(behaviors)

def add_content_type(context, fti_id, profile):
    fti = add_dexterity_content_type_to_portal_registryfti_id(context, fti_id)
    type_info_import_step(context, fti, profile)

def type_info_import_step(context, fti, profile):
    stool = getToolByName(context, 'portal_setup')
    directory_import = stool._getImportContext(profile)
    importObjects(fti, "types/", directory_import)

def add_dexterity_content_type_to_portal_registryfti_id(context, fti_id):
    if not HAS_DEXTERITY:
        raise NotImplementedError()
    fti = DexterityFTI(fti_id)
    fti.id = fti_id
    ttool = getToolByName(context, 'portal_types')
    if fti_id in ttool:
        logger.warning("%s type already exists!", fti_id)
    else:
        ttool._setObject(fti.id, fti)
    return ttool[fti_id]
```
