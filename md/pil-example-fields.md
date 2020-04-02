---
title: pil example fields (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'fields'


## python fields

Python pil example: fields

```python
from django import forms
from django.utils.translation import ugettext_lazy as _
from django.utils.datastructures import  MultiValueDict, MergeDict
from django.core import validators
from django.core.exceptions import ValidationError

try:
    from cStringIO import StringIO
except ImportError:
    from StringIO import StringIO

FILE_INPUT_CONTRADICTION = object()

class MultipleClearableFileInput(forms.ClearableFileInput):

    def render(self, name, value, attrs=None):
        attrs = attrs or {}
        attrs['multiple'] = 'multiple'
        return super(MultipleClearableFileInput, self).render(name, value, attrs)

    def value_from_datadict(self, data, files, name):
        if isinstance(files, (MultiValueDict, MergeDict)):
            return files.getlist(name)
        return files.get(name, None)

class MultipleFileField(forms.Field):
    widget = MultipleClearableFileInput
    default_error_messages = {
        'invalid': _(u"No file was submitted. Check the encoding type on the form."),
        'missing': _(u"No file was submitted."),
        'empty': _(u"The submitted file '%(name)s' is empty."),
        'max_length': _(u"Ensure the file '%(name)s' has at most %(max)d characters (it has %(length)d)."),
        'contradiction': _(u"Please either submit a file or check the clear checkbox, not both.")
    }

    def __init__(self, *args, **kwargs):
        self.max_length = kwargs.pop('max_length', None)
        self.allow_empty_file = kwargs.pop('allow_empty_file', False)
        super(MultipleFileField, self).__init__(*args, **kwargs)

    def to_python(self, data):
        if data in validators.EMPTY_VALUES:
            return None
        messages = []
        for d in data:
            try:
                # UploadedFile objects should have name and size attributes.
                try:
                    file_name = d.name
                    file_size = d.size
                except AttributeError:
                    raise ValidationError(self.error_messages['invalid'])

                if self.max_length is not None and len(file_name) > self.max_length:
                    error_values =  {'max': self.max_length, 'length': len(file_name), 'name': d.name}
                    raise ValidationError(self.error_messages['max_length'] % error_values)
                if not file_name:
                    raise ValidationError(self.error_messages['invalid'])
                if not self.allow_empty_file and not file_size:
                    raise ValidationError(self.error_messages['empty'] % {'name': d.name})
            except ValidationError as ve:
                messages += ve.messages
        if messages:
            raise ValidationError(messages)
        return data

    def clean(self, data, initial=None):
        # If the widget got contradictory inputs, we raise a validation error
        if data is FILE_INPUT_CONTRADICTION:
            raise ValidationError(self.error_messages['contradiction'])
            # False means the field value should be cleared; further validation is
        # not needed.
        if data is False:
            if not self.required:
                return False
                # If the field is required, clearing is not possible (the widget
            # shouldn't return False data in that case anyway). False is not
            # in validators.EMPTY_VALUES; if a False value makes it this far
            # it should be validated from here on out as None (so it will be
            # caught by the required check).
            data = None
        if not data and initial:
            return initial
        return super(MultipleFileField, self).clean(data)

    def bound_data(self, data, initial):
        if data in (None, FILE_INPUT_CONTRADICTION):
            return initial
        return data

class MultipleImageField(MultipleFileField):
    default_error_messages = {
        'invalid_image': _(u"Upload a valid image. The file '%(name)s' you uploaded was either not an image or a corrupted image."),
        }

    def to_python(self, data):
        """
        Checks that the file-upload field data contains a valid image (GIF, JPG,
        PNG, possibly others -- whatever the Python Imaging Library supports).
        """
        data = super(MultipleImageField, self).to_python(data)
        if data in validators.EMPTY_VALUES:
            return None
        fs = []
        messages = []
        for d in data:
            try:
                if d is None:
                    continue

                # Try to import PIL in either of the two ways it can end up installed.
                try:
                    from PIL import Image
                except ImportError:
                    import Image

                # We need to get a file object for PIL. We might have a path or we might
                # have to read the data into memory.
                if hasattr(d, 'temporary_file_path'):
                    file = d.temporary_file_path()
                else:
                    if hasattr(d, 'read'):
                        file = StringIO(d.read())
                    else:
                        file = StringIO(d['content'])

                try:
                    # load() is the only method that can spot a truncated JPEG,
                    #  but it cannot be called sanely after verify()
                    trial_image = Image.open(file)
                    trial_image.load()

                    # Since we're about to use the file again we have to reset the
                    # file object if possible.
                    if hasattr(file, 'reset'):
                        file.reset()

                    # verify() is the only method that can spot a corrupt PNG,
                    #  but it must be called immediately after the constructor
                    trial_image = Image.open(file)
                    trial_image.verify()
                except ImportError:
                    # Under PyPy, it is possible to import PIL. However, the underlying
                    # _imaging C module isn't available, so an ImportError will be
                    # raised. Catch and re-raise.
                    raise
                except Exception: # Python Imaging Library doesn't recognize it as an image
                    raise ValidationError(self.error_messages['invalid_image'] % {'name': d.name})
                if hasattr(d, 'seek') and callable(d.seek):
                    d.seek(0)
                fs.append(d)
            except ValidationError as ve:
                messages += ve.messages
        if messages:
            raise ValidationError(messages)
        return fs

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
