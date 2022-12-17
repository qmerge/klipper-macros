Updated mods to printer.cfg

#   Use PRINT_END for the slicer ending script - please customise for your slicer of choice
gcode:
    # safe anti-stringing move coords
    {% set th = printer.toolhead %}
    {% set x_safe = th.position.x + 20 * (1 if th.axis_maximum.x - th.position.x > 20 else -1) %}
    {% set y_safe = th.position.y + 20 * (1 if th.axis_maximum.y - th.position.y > 20 else -1) %}
    {% set z_safe = [th.position.z + 2, th.axis_maximum.z]|min %}
	M117 Print_End
    
    SAVE_GCODE_STATE NAME=STATE_PRINT_END
    
    M400                           ; wait for buffer to clear
    G92 E0                         ; zero the extruder
    G1 E-5.0 F3600                 ; retract filament
    
    TURN_OFF_HEATERS
    
    G90                                      ; absolute positioning
    G0 X{x_safe} Y{y_safe} Z{z_safe} F20000  ; move nozzle to remove stringing
    G0 X{th.axis_maximum.x//2} Y{th.axis_maximum.y - 2} F3600  ; park nozzle at rear
    M107                                     ; turn off fan
    
    #BED_MESH_CLEAR
    RESTORE_GCODE_STATE NAME=STATE_PRINT_END
# All customizations are documented in globals.cfg. Just copy a variable from
# there into the section below, and change the value to meet your needs.

[gcode_macro _km_options]
# These are examples of some likely customizations:
# Any sheets in the below list will be available with a configurable offset.
variable_bed_surfaces: ['smooth_1','texture_1']
# Length (in mm) of filament to load (bowden tubes will be longer).
variable_load_length: 90.0
# Hide the Octoprint LCD menu since I don't use it.
variable_menu_show_octoprint: False
#hid the sd card menu because it doesn't work
variable_menu_show_sdcard: False
# Customize the filament menus (up to 10 entries).
variable_menu_temperature: [
  {'name' : 'PLA',  'extruder' : 200.0, 'bed' : 60.0},
  {'name' : 'PETG', 'extruder' : 230.0, 'bed' : 85.0},
#  {'name' : 'ABS',  'extruder' : 245.0, 'bed' : 110.0, 'chamber' : 60}]
  {'name' : 'ABS',  'extruder' : 245.0, 'bed' : 110.0}]
gcode: # This line is required by Klipper.
# Any code you put here will run at klipper startup, after the initialization
# for these macros. For example, you could uncomment the following line to
# automatically adjust your bed surface offsets to account for any changes made
# to your Z endstop or probe offset.
#  ADJUST_SURFACE_OFFSETS

# This line includes all the standard macros.
[include klipper-macros/*.cfg]
# Uncomment to include features that require specific hardware support.
# LCD menu support for features like bed surface selection and pause next layer.
[include klipper-macros/optional/lcd_menus.cfg]
# Optimized bed leveling
#[include klipper-macros/optional/bed_mesh.cfg]
[gcode_macro center]
gcode:
	G90
	G0 X125 Y125 F6000
# The sections below here are required for the macros to work.
[idle_timeout]
gcode:
  _KM_IDLE_TIMEOUT

[pause_resume]

[respond]

[save_variables]
filename: ~/variables.cfg # UPDATE THIS FOR YOUR PATH!!!

[virtual_sdcard]
path: ~/.octoprint/virtualSd/

#debug setting
[respond]

#end Updated mods to printer.cfg

Macro's that work
park.cfg:[gcode_macro park]
pause_resume_cancel.cfg:[gcode_macro pause]
pause_resume_cancel.cfg:[gcode_macro m600]
pause_resume_cancel.cfg:[gcode_macro resume]
filament.cfg:[gcode_macro load_filament]
filament.cfg:[gcode_macro unload_filament]

Macro's that Don't work

Send: pause_next_layer
Recv: !! Error evaluating 'gcode_macro gcode_at_layer:gcode': TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'bed_mesh_fast.cfg:[gcode_macro bed_mesh_calibrate_fast]

Macro's that may not work.

bed_mesh_fast.cfg:[gcode_macro bed_mesh_check]
bed_surface.cfg:[gcode_macro _apply_bed_surface_offset]
bed_surface.cfg:[gcode_macro _init_surfaces]
bed_surface.cfg:[gcode_macro adjust_surface_offsets]
bed_surface.cfg:[gcode_macro set_surface_offset]
bed_surface.cfg:[gcode_macro set_surface_active]
bed_surface.cfg:[gcode_macro set_gcode_offset]
beep.cfg:[gcode_macro m300]
fans.cfg:[gcode_macro _check_fan_params]
fans.cfg:[gcode_macro set_fan_scaling]
fans.cfg:[gcode_macro reset_fan_scaling]
fans.cfg:[gcode_macro m106]
filament.cfg:[gcode_macro _load_unload]

filament.cfg:[gcode_macro _pause_inner_m700]
filament.cfg:[gcode_macro m701]
filament.cfg:[gcode_macro m702]
globals.cfg:[gcode_macro _km_globals]
globals.cfg:[gcode_macro _check_globals]
globals.cfg:[gcode_macro kmvars]
globals.cfg:[gcode_macro check_macro_docs]
globals.cfg:[gcode_macro listvars]
globals.cfg:[gcode_macro listvarnames]
heaters.cfg:[gcode_macro set_heater_temperature_scaled]
heaters.cfg:[gcode_macro temperature_wait_scaled]
heaters.cfg:[gcode_macro _gcode_wait_wrapper_inner]
heaters.cfg:[gcode_macro _gcode_wait_wrapper]
heaters.cfg:[gcode_macro m109]
heaters.cfg:[gcode_macro m190]
heaters.cfg:[gcode_macro m191]
heaters.cfg:[gcode_macro m104]
heaters.cfg:[gcode_macro m140]
heaters.cfg:[gcode_macro m141]
heaters.cfg:[gcode_macro _check_heater_params]
heaters.cfg:[gcode_macro set_heater_scaling]
heaters.cfg:[gcode_macro reset_heater_scaling]
idle.cfg:[gcode_macro _km_idle_timeout]
kinematics.cfg:[gcode_macro _check_kinematic_limits]
kinematics.cfg:[gcode_macro g28]
layers.cfg:[gcode_macro before_layer_change]
layers.cfg:[gcode_macro after_layer_change]
layers.cfg:[gcode_macro gcode_at_layer]
layers.cfg:[gcode_macro init_layer_gcode]
layers.cfg:[gcode_macro _reset_layer_gcode]
layers.cfg:[gcode_macro cancel_all_layer_gcode]
layers.cfg:[gcode_macro pause_next_layer]
layers.cfg:[gcode_macro _km_layer_run]
layers.cfg:[gcode_macro pause_at_layer]
layers.cfg:[gcode_macro speed_at_layer]
layers.cfg:[gcode_macro flow_at_layer]
layers.cfg:[gcode_macro fan_at_layer]
layers.cfg:[gcode_macro heater_at_layer]
park.cfg:[gcode_macro _park_inner]
park.cfg:[gcode_macro g27]
pause_resume_cancel.cfg:[gcode_macro m601]
pause_resume_cancel.cfg:[gcode_macro m602]
pause_resume_cancel.cfg:[gcode_macro m24]
pause_resume_cancel.cfg:[gcode_macro m25]
pause_resume_cancel.cfg:[gcode_macro cancel_print]
pause_resume_cancel.cfg:[gcode_macro clear_pause]
start_end.cfg:[gcode_macro print_start]
start_end.cfg:[gcode_macro print_start_set]
start_end.cfg:[gcode_macro print_end]
state.cfg:[gcode_macro _km_save_state]
state.cfg:[gcode_macro save_gcode_state]
state.cfg:[gcode_macro restore_gcode_state]
velocity.cfg:[gcode_macro m201]
velocity.cfg:[gcode_macro m203]
velocity.cfg:[gcode_macro m204]
velocity.cfg:[gcode_macro m205]
velocity.cfg:[gcode_macro m900]
velocity.cfg:[gcode_macro _reset_velocity_limits]
