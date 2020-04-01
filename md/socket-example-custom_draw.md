---
title: socket example custom draw (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'custom draw'

Functions in program: 
* `def unregister():`
* `def register():`

Modules used in program: 
* `import bpy`

## python custom draw

Python socket example: custom draw

```python
import bpy
from bpy.props import FloatVectorProperty
from bpy.props import FloatProperty, BoolProperty
from sverchok.node_tree import SverchCustomTreeNode
from sverchok.data_structure import updateNode
from sverchok.core.socket_data import SvGetSocketInfo

socket_colors = {
    "COLOR": (0.899, 0.8052, 0.0, 1.0),
    "QUATERNION": (0.9, 0.4, 0.7, 1.0)
 }


class SvFKB(bpy.types.Node, SverchCustomTreeNode):
    ''' a SvFKB f '''
    bl_idname = 'SvFKB'
    bl_label = 'SvFKB _ :)'
    bl_icon = 'GREASEPENCIL'

    quaternion_a = FloatVectorProperty(subtype='QUATERNION', min=0, max=0, size=4, default=(0,0,0,1))
    expand_a = BoolProperty()
    expand_b = BoolProperty()

    def sv_init(self, context):
        A = self.inputs.new('StringsSocket', "Float")
        A.prop_name = 'quaternion_a'
        A.custom_draw = 'draw_one_socket'
        A.nodule_color = socket_colors['QUATERNION']
        OUT = self.outputs.new('StringsSocket', "Float")
        OUT.nodule_color = socket_colors['QUATERNION']

    def draw_buttons(self, context, layout):
        layout.label('helios')

    def draw_one_socket(self, socket, context, layout):
        node = self
        if not socket.is_linked:
            c = layout.column(align=True)
            c.prop(node, socket.prop_name, text='')
        else:
            if socket.prop_name:
                prop = node.rna_type.properties.get(socket.prop_name, None)
                t = prop.name.split('_')[1].upper()
            else:
                t=''
            layout.label(t + '. ' + SvGetSocketInfo(socket))

    def process(self):
        _in = self.inputs[0].sv_get()
        self.outputs[0].sv_set(_in)


def register():
    bpy.utils.register_class(SvFKB)


def unregister():
    bpy.utils.unregister_class(SvFKB)


if __name__ == '__main__':
    register()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
