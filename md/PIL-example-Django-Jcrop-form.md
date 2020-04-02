---
title: PIL example Django-Jcrop-form (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Django-Jcrop-form'

Functions in program: 
* `def img_edit_view(request):`

Modules used in program: 
* `import Image as pil`

## python Django-Jcrop-form

Python PIL example: Django-Jcrop-form

```python
"""
Django Jcrop form

Author: Markus Thielen, mt[aaTt]thiguten.de
Comment: MicFan, fanlinsheng{AATT}gmail.com

http://djangosnippets.org/snippets/2316/

= Description =

Implements a Django form that integrates image uploading plus cropping using
the awesome Jcrop plugin (http://deepliquid.com/content/Jcrop.html).

It does not create or use any models, so to use it, simply copy it to
your project tree and import it as appropriate.

= License =

MIT

= Documention =

http://pillow.readthedocs.org/en/latest/index.html

= Install =

sudo apt-get build-dep python-imaging
sudo apt-get install python-dev
wget -q -O - https://raw.github.com/gist/1225180/patch.sh | sudo bash
sudo ln -sf /usr/lib/i386-linux-gnu/libfreetype.so /usr/lib/
sudo ln -sf /usr/lib/i386-linux-gnu/libz.so /usr/lib/
sudo ln -sf /usr/lib/i386-linux-gnu/libjpeg.so /usr/lib/
pip install -I PIL

    --------------------------------------------------------------------
    PIL 1.1.7 SETUP SUMMARY
    --------------------------------------------------------------------
    version       1.1.7
    platform      linux2 2.7.4 (default, Apr 19 2013, 18:32:33)
                  [GCC 4.7.3]
    --------------------------------------------------------------------
    *** TKINTER support not available (Tcl/Tk 8.5 libraries needed)
    --- JPEG support available
    *** ZLIB (PNG/ZIP) support not available
    --- FREETYPE2 support available
    *** LITTLECMS support not available
    --------------------------------------------------------------------

= Limitations =

Many, probably ;-) It does basically work but lacks proper error handling.
If you upload something that is not an image, no error is dispayed.
I hope I find the time to fix this. If you have a hint, I'd be grateful if you
dropped me a note.

= Usage =

In your views.py, import JcropForm.
The view function that displays the form has three parts (or control flows):

 * if request is POSTed but contains no uploaded files, crop coordinates
   were submitted. Use JcropForm's crop/resize/save methods to apply.
 * if request was posted with file data, the user just uploaded a new
   image. Use JcropForm's static method prepare_uploaded_img to scale the
   image to a reasonable size (that does not break your layout) and save it
 * for normal GET requests just display the form with the current image.


Example view function:

@login_required # the view func expects a logged on user
def img_edit_view(request):
  # get the profile (i.e. the model containing the image to edit);
  # In this example, the model in question is the user profile model,
  # so we can use Django's get_profile() method.
  profile = request.user.get_profile()
  
  # define a fixed aspect ratio for the user image
  aspect = 105.0 / 75.0
  # the final size of the user image
  final_size = (105, 75)
  
  if request.method == "POST" and len(request.FILES) == 0:
    # user submitted form with crop coordinates
    form = JcropForm(request.POST)
    if form.is_valid():
      # apply cropping
      form.crop()
      form.resize(final_size)
      form.save()
      # redirect to profile display page
      return HttpResponseRedirect("/myprofile/")
    
  elif request.method == "POST" and len(request.FILES):
    # user uploaded a new image; save it and make sure it is not too large
    # for our layout
    img_fn = JcropForm.prepare_uploaded_img(request.FILES, image_upload_to, 
                                            profile, (370, 500))
    if img_fn:
      # store new image in the member instance
      profile.avatar = img_fn # 'avatar' is an ImageField
      profile.save()
      
      # redisplay the form with the new image; this is the same as for
      # GET requests -> fall through to GET
      
  elif request.method != "GET":
    # only POST and GET, please
    return HttpResponse(status=400)
  
  # for GET requests, just display the form with current image
  form = JcropForm(initial        = { "imagefile": profile.avatar },
                   jcrop_options  = { 
                                      "aspectRatio":aspect,
                                      "setSelect": "[100, 100, 50, 50]",
                                    }
                  )

  return render_to_response("profile/img_edit.html",
                            {
                              "form": form,
                            },
                            RequestContext(request))


The template is the same as for normal Django forms, nothing special there.

This code is somehow inspired by https://github.com/azizmb/django-ip-form,
although the original code did not work for me.

"""
from django import forms
from django.conf import settings
from django.utils.safestring import mark_safe
from django.utils.datastructures import MultiValueDictKeyError
import Image as pil

UPLOAD_IMG_ID="new-img-file"

class JcropWidget(forms.Widget):
  class Media:
    # form media, i.e. CSS and JavaScript needed for Jcrop.
    # You'll have to adopt these to your project's paths.
    css = {
      'all': (settings.MEDIA_URL + "css/jquery.Jcrop.css",)
    }
    js = (
      settings.MEDIA_URL + "js/lib/jquery.Jcrop.min.js",
    )
  
  # fixed Jcrop options; to pass options to Jcrop, use the jcrop_options
  # argument passed to the JcropForm constructor. See example above.
  jcrop_options = {
                    "onSelect": "storeCoords", 
                    "onChange": "storeCoords",
                  }
  
  # HTML template for the widget. 
  #
  # The widget is constructed from the following parts:
  #
  #  * HTML <img> - the actual image used for displaying and cropping
  #  * HTML <label> and <input type="file> - used for uploading a new
  #                                          image
  #  * HTML <input type="hidden"> - to remember image path and filename
  #  * JS code - The JS code makes the image a Jcrop widget and 
  #              registers an event handler for the <input type="file"> 
  #              widget. The event handler submits the form so the new
  #              image is sent to the server without the user having
  #              to press the submit button.
  # 
  markup = """
  <img id="jcrop-img" src="%(MEDIA_URL)s%(img_fn)s"/><br/>
  <label for="new-img-file">Neues Bild hochladen:</label>
  <input type="file" name="%(UPLOAD_IMG_ID)s" id="%(UPLOAD_IMG_ID)s"/>
  <input type="hidden" name="imagefile" id="imagefile" value="%(imagefile)s"/>
  <script type="text/javascript">
  function storeCoords(c)
  {
    jQuery('#id_x1').val(c.x);
    jQuery('#id_x2').val(c.x2);
    jQuery('#id_y1').val(c.y);
    jQuery('#id_y2').val(c.y2);
  }
  jQuery(function() {
      jQuery('#jcrop-img').Jcrop(%(jcrop_options)s);
      jQuery('#%(UPLOAD_IMG_ID)s').change(function(e){
        var form = jQuery('#%(UPLOAD_IMG_ID)s').parents('form:first');
        form.submit();
      });
  });</script>
    """

  def __init__(self, attrs=None):
    """
    __init__ does nothing special for now
    """
    super(JcropWidget, self).__init__(attrs)
    
  def add_jcrop_options(self, options):
    """
    add jcrop options; options is expected to be a dictionary of name/value
    pairs that Jcrop understands; 
    see http://deepliquid.com/content/Jcrop_Manual.html#Setting_Options
    """
    for k, v in options.items():
      self.jcrop_options[k] = v
    
  def render(self, name, value, attrs=None):
    """
    render the Jcrop widget in HTML
    """
    # translate jcrop_options dictionary to JavaScipt
    jcrop_options = "{";
    for k, v in self.jcrop_options.items():
      jcrop_options = jcrop_options + "%s: %s," % (k, v)
    jcrop_options = jcrop_options + "}"
    
    # fill in HTML markup string with actual data
    output = self.markup % {
                             "MEDIA_URL": settings.MEDIA_URL, 
                             "img_fn": str(value),
                             "UPLOAD_IMG_ID": UPLOAD_IMG_ID,
                             "jcrop_options": jcrop_options,
                             "imagefile": value,
                           }
    return mark_safe(output)

    
class JcropForm(forms.Form):
  """
  Jcrop form class
  """
  imagefile = forms.Field(widget=JcropWidget(), label="", required=False)
  x1 = forms.DecimalField(widget=forms.HiddenInput)
  y1 = forms.DecimalField(widget=forms.HiddenInput)
  x2 = forms.DecimalField(widget=forms.HiddenInput)
  y2 = forms.DecimalField(widget=forms.HiddenInput)
    
  def __init__(self, *args, **kwargs):
    """
    overridden init func; check for Jcrop options and remove them
    from kwargs
    """    
    # remove upload image post data (if present); this would make Django form
    # code hick up (since there is no upload image widget in the control)...
    try:
      post_data = args[0]
      if UPLOAD_IMG_ID in post_data:
        del post_data[UPLOAD_IMG_ID]
    except (IndexError):
      # no POST data passed; nothing todo anyway
      pass
  
    jcrop_options = {}
    if "jcrop_options" in kwargs:
      jcrop_options = kwargs["jcrop_options"]
      del(kwargs["jcrop_options"])
  
    # call base class __init__
    super(JcropForm, self).__init__(*args, **kwargs)
  
    # set Jcrop options for our crop widget 
    self.fields["imagefile"].widget.add_jcrop_options(jcrop_options)
    
  def clean_imagefile(self):
    """
    instantiate PIL image; raise ValidationError if field contains no image
    """
    try:
      self.img = pil.open(settings.MEDIA_ROOT + self.cleaned_data["imagefile"])
    except IOError:
      raise forms.ValidationError("Invalid image file")
    return self.cleaned_data["imagefile"]
  
  
  def is_valid(self):
    """
    checks if self._errors is empty; if so, self._errors is set to None and
    full_clean() is called.
    This is necessary since the base class' is_valid() method does
    not populate cleaned_data if _errors is an empty ErrorDict (but not 'None').
    I just failed to work this out by other means...
    """
    if self._errors is not None and len(self._errors) == 0:
      self._errors = None
      self.full_clean()
    return super(JcropForm, self).is_valid()

  def crop (self):
    """
    crop the image to the user supplied coordinates
    """
    x1=self.cleaned_data['x1']
    x2=self.cleaned_data['x2']
    y1=self.cleaned_data['y1']
    y2=self.cleaned_data['y2']
    self.img = self.img.crop((x1, y1, x2, y2))

  def resize (self, dimensions, maintain_ratio=False):
    """
    resize image to dimensions passed in
    """
    if maintain_ratio:
      self.img = self.img.thumbnail(dimensions, pil.ANTIALIAS)
    else:
      self.img = self.img.resize(dimensions, pil.ANTIALIAS)

  def save(self):
    """
    save image...
    """
    self.img.save(settings.MEDIA_ROOT + self.cleaned_data['imagefile'])

  @staticmethod
  def prepare_uploaded_img(files, upload_to, profile, max_display_size=None):
    """
    stores an uploaded image in the proper destination path and 
    optionally resizes it so it can be displayed properly.
    Returns path and filename of the new image (without MEDIA_ROOT).
    
    'upload_to' must be a function reference as expected by Django's
    FileField object, i.e. a function that expects a profile instance
    and a file name and that returns the final path and name for the
    file. 
    """
    try:
      upload_file = files[UPLOAD_IMG_ID]
    except MultiValueDictKeyError:
      # files dict does not contain new image
      return None
    
    # copy image data to final file
    fn = upload_to(profile, upload_file.name)
    pfn = settings.MEDIA_ROOT + fn
    destination = open(pfn, 'wb+')
    for chunk in upload_file.chunks():
      destination.write(chunk)
    destination.close()
    
    if max_display_size:
      # resize image if larger than specified
      im = pil.open(pfn)
      if im.size[0] > max_display_size[0]:
        # image is wider than allowed; resize it
        im = im.resize((max_display_size[0], 
                        im.size[1] * max_display_size[0] / im.size[0]),
                        pil.ANTIALIAS)
      if im.size[1] > max_display_size[1]:
        # image is taller than allowed; resize it
        im = im.resize((im.size[0] * max_display_size[1] / im.size[1], 
                        im.size[1]), pil.ANTIALIAS)
      im.save(pfn)
    
    return fn
        


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
