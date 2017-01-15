## x_axis_motor_mount
For me, the stock x-axis mount had two issues:   
- The holes were such that the ballscrew was shifted 2.5mm off centerline, causing binding.  (Apparently some LMS3990s were made in such a way that required this shift.  Not mine.)  
- The bottom of the mount interferes with my y-axis DRO, limiting travel of the x-axis.

The model was designed in FreeCAD. Exported as STEP model.  
CAM was done in HeeksCNC.  
Files manually separated, one per tool.  Hand edited.  
The [Helical Boring G-Code generator](https://www.chiefdelphi.com/forums/showthread.php?postid=1187970) was used to generate the deep holes.  

I should note there are some problems with this flow:  
- Heeks wasn't able to import the motor screw holes from the STEP model.  They were seen as edges, not circles.  
- LinuxCNC manual tool changes with tool-touch-off doesn't work so well in g-code.  Hence, separate files per tool.  

This was my first CNC project, so I ended up iterating on the CAM several times to get things dialed in right for my mill.

## Operations
Operations:  
- before loading material, home axes
- use 2 inch thick stock.
- Adjust stock location to make sure we're within machine limits
- center the vise X axis around +1 inch to give clearance for the -X limit.  
- secure stock in vise with top 40 mm or greater exposed
- 0,0 pos is 0.5mm, 1mm, 1mm into material
- Make sure tool table matches the CAM program.
- Manually switch to the first tool: MDI command T4 M06 G43.
- Tool-touch-off top of block as z=1mm
- drill first, before facing, so we can run 3 subsequent ops without changing tools.  
- Load next program.
- Manually switch to the next tool: MDI command T6 M06 G43.
- Switch tool
- Tool-touch-off top of block as z=1mm
- Note counterbore Z=0 is cutting face. Clearance needed to be increased +7 mm to accomodate pilot.

- Try Helical Boring G-Code generator here: https://www.chiefdelphi.com/forums/showthread.php?postid=1187970
  - Note, the 'final pass' is full-depth cut - won't work since our hole is larger dia than the cutter.
  - Note, it assumes inches and drops to 0.005 of the surface before starting helix.  Fix that.
- Use Helical Boring Generator to make R19.15 x 2mm Shoulder    @ X60.25 Y-37.0 Z0
- Use Helical Boring Generator to make R15.875 x 37mm Coupler   @ X60.25 Y-37.0 Z-2.0

- Backside ops: mirrored the part in HeeksCNC. Set new 0,0,0 position at the vise side of the part.
- Backside facing and pocket ops extend beyond the x-extents of the part, so x doesn't need good alignment.  Y, Z do.
- Touch off Y=0 from vise edge before mounting part
- Touch off tool Z=0 from the parallels, without the part mounted in vise yet.  Or from vise top, accounting for height of vise minus the parallels.

## Results
See the first_try_front.jpg and first_try_back.jpg. These are from the first part I made. Surface finish is pretty poor, and I had to take a file to it in several places. I made a mistake after a tool change and the end mill crashed into the top surface, leaving a defect. There was a lot of x-axis backlash and the center hole came out undersized.  I had to recut that a second time to allow the motor to fit. The lead-in/lead-out during the profiling operation left a mark on the top surface, and that surface was not flat. Not sure yet if this was due to missed steps or something else.  45-deg cuts had a rippled appearance. Dimensions on X were undersized.  Dimensions on Y and Z were accurate within ~6 mil.  Overall quality was poor, but the fit was sufficient to install and use. 

After installing the mount, it seems to function well. X-axis no longer binds, and I can fully tighten the ballnut set screw to improve backlash. There is no interference with the DRO mounting rail, so I can now achieve over 12 inch table travel in X.  For my very first CNC part, it wasn't too bad.
