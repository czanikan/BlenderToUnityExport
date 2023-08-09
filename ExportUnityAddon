bl_info = {
    "name": "Export to Unity",
    "author": "Czanik András",
    "blender": (2, 80, 0),
    "category": "Import-Export",
}

import addon_utils
import bpy
import os
from pathlib import Path
from datetime import datetime

class ObjectExportToUnity(bpy.types.Operator):
    """Object Export To Unity"""
    bl_idname = "export.export_to_unity"
    bl_label = "Export To Unity"
    bl_options = {'REGISTER', 'UNDO'}

    def execute(self, context):
        now = datetime.now()
        current_time = now.strftime("%m%d%H%M%S")
        
        name = "export" + current_time
        home_directory = Path.home()
        print(f"Home directory: {home_directory}")
        
        # Create folder if it doesn't exist
        directory = os.path.join(home_directory, "Desktop/FBX")
        if not os.path.exists(directory):
            os.makedirs(directory)

        
        relative_path = directory + "/" + name + ".fbx"
        absolute_path = os.path.join(home_directory, relative_path)
        
        bpy.ops.object.origin_set(type='ORIGIN_GEOMETRY')
        bpy.ops.export_scene.fbx(   filepath=absolute_path,
                                    use_selection=True,
                                    object_types={'MESH'}, 
                                    check_existing=True,
                                    bake_space_transform=True,
                                    apply_scale_options='FBX_SCALE_ALL')
                                    
        return {'FINISHED'}
    
# store keymaps here to access after registration
addon_keymaps = []

def menu_func(self, context):
    self.layout.operator(ObjectExportToUnity.bl_idname)                                

def register():
    bpy.utils.register_class(ObjectExportToUnity)
    bpy.types.VIEW3D_MT_object.append(menu_func)
    
    # handle the keymap
    wm = bpy.context.window_manager
    # Note that in background mode (no GUI available), keyconfigs are not available either,
    # so we have to check this to avoid nasty errors in background case.
    kc = wm.keyconfigs.addon
    if kc:
        km = wm.keyconfigs.addon.keymaps.new(name='Export Mode', space_type='EMPTY')
        kmi = km.keymap_items.new(ObjectExportToUnity.bl_idname, 'E', 'PRESS', ctrl=True, shift=True)
        addon_keymaps.append((km, kmi))

def unregister():
    for km, kmi in addon_keymaps:
        km.keymap_items.remove(kmi)
    addon_keymaps.clear()

    bpy.utils.unregister_class(ObjectExportToUnity)
    bpy.types.VIEW3D_MT_object.remove(menu_func)


if __name__ == "__main__":
    register()