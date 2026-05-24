# Shared Airtable Movement Library — Final Schema
Version: 1.0
Last Updated: May 23, 2026
Status: Locked — pending Airtable build and app implementation

---

## CORE IDENTITY FIELDS

| Field | Type | Notes |
|---|---|---|
| Movement Name | Text | Canonical, consistent across all apps |
| aka / Aliases | Multi-line text | Word-level search array |
| Description | Short text | Brief summary of movement |
| Coaching Notes | Long text | Long-form cueing, client-visible |
| Demo Video URL | URL | YouTube or other |
| display_priority | Number | Default 999, controls picker sort order |
| Skill/Complexity Rating | Number | 1–5 scale |
| provisional_status | Single-select | approved, pending_approval — visible immediately, admin reviews async |
| created_by | Text | User ID of submitter |
| is_deprecated | Checkbox | Soft-delete only, never hard delete |
| default_frequency | Decimal | Times per week e.g. 1.3, 0.33 |

---

## CLASSIFICATION FIELDS

### Movement Category — single-select (pick the dominant physical demand)
Power, Strength, Gymnastics, Plyometric, Mobility/Flexibility, Locomotion, Balance, Other

### Movement Category Tags — multi-select
Isolation, Isometric, Accessory, Warm-Up, Cool-Down, Speed, Accuracy, Coordination, Weightlifting

---

### Equipment (Primary) — single-select
Barbell, Bumper Plate, Bands, Bike, Bodyweight, Box, Cable Machine, Dumbbell, GHD, Jump Rope, Kettlebell, Machine, Medicine Ball, Pull-Up Bar, Rings, Rope, Rower, Sandbag, Ski Erg, Sled, Slam Ball, Squat Rack, Suspension Trainer, Wall Ball, Yoke, Other

### Equipment Tags — multi-select
AbMat, Bench, Bosu Ball, Crossover Symmetry, Foam Roller, Knee Pad, Lacrosse Ball, Machine-Assisted, Machine-Cable, Machine-Pin Selector, Machine-Plate Loaded, PVC Pipe, Rig, TRX, Yoga Mat, Other

---

### Primary Movement Pattern — single-select
Squat, Hinge, Lunge, Step-Up, Horizontal Push, Vertical Push, Horizontal Pull, Vertical Pull, Carry, Rotation, Anti-Rotation, Core Flexion, Core Extension, Core Stabilization, Thoracic Spine, Neck, Jump, Locomotion, Other

### Joint Action Tags — multi-select
Hip: flexion, extension, abduction, adduction, internal rotation, external rotation
Knee: flexion, extension
Ankle: plantar flexion, dorsiflexion
Shoulder: flexion, extension, abduction, adduction, internal rotation, external rotation
Scapular: elevation, depression, retraction, protraction, upward rotation, downward rotation
Elbow/Forearm: flexion, extension, pronation, supination
Wrist: flexion, extension, radial deviation
Spine/Core: flexion, extension, rotation, anti-rotation, lateral flexion, anti-lateral flexion, stabilization
Neck: flexion, extension, rotation, lateral flexion, stabilization

---

### Primary Muscle Group — multi-select
Abs, Back-Upper, Back-Middle, Back-Lower, Biceps, Calves, Chest, Glutes, Grip/Forearms, Hamstrings, Hip Abductors, Hip Adductors, Hip Flexors, Lats, Quads, Shoulders, Tibs, Traps, Triceps, Delts-Front, Delts-Rear, Delts-Side

### Specific Muscle Tags — multi-select
Pec-major, Pec-minor, Serratus anterior, Delt-front, Delt-side, Delt-rear, Rotator cuff, Teres-major, Teres-minor, Trap-upper, Trap-middle, Trap-lower, Lats, Rhomboids, Spinal erectors, QL, Biceps, Brachialis, Brachioradialis, Triceps, Forearms-flexors, Forearms-extensors, Core-rectus abdominis, Core-obliques, Core-transverse abdominis, Glutes-max, Glutes-med, Glutes-min, Hips-flexors, Hips-adductors, Hips-abductors, Hip-deep external rotators, Quad-rectus femoris, Quad-vastus medialis, Quad-vastus lateralis, Quad-vastus intermedius, Hamstrings-lateral, Hamstrings-medial, Calves-gastroc, Calves-soleus, Lower leg-tibialis anterior, Lower leg-peroneals

---

## BEHAVIORAL FLAGS

| Field | Type | Notes |
|---|---|---|
| strength_trackable | Checkbox | Tracks weight x reps; 1RM/volume/PR analytics |
| is_paired | Checkbox | Two implements (2 KBs, 2 DBs); weight is per piece |
| time_based | Checkbox | Measured in seconds |
| is_distance | Checkbox | Logs distance; unit chosen per set |
| is_calorie | Checkbox | Logs calories |
| is_height | Checkbox | Logs height in inches |
| is_band_only | Checkbox | Band is the resistance; no weight field |
| is_bodyweight | Checkbox | Bodyweight is the load |
| is_assisted | Checkbox | Weight subtracts from bodyweight |
| unilateral | Checkbox | Performed one side at a time |
| machine_type | Single-select | pin_selector, plate_loaded, free_motion_cable |
| is_smash | Checkbox | Soft tissue/therapeutic; forces time_based, clears strength_trackable |

---

## EXECUTION & LOAD OPTIONS

| Field | Type | Notes |
|---|---|---|
| allowed_executions | Multi-select | Admin-set at creation; default = no restrictions. Options: Single-Arm, Single-Leg, Alternating, Both Arms, Both Legs, Other |
| allowed_loads | Multi-select | Admin-set at creation; default = no restrictions. Options: 1 KB, 2 KB, 1 DB, 2 DB, Barbell, Bodyweight, Band, Other |
| default_distance_unit | Single-select | feet, meters, miles, yards |

---

## RELATIONSHIPS

| Field | Type | Notes |
|---|---|---|
| parent_id / Variant Group | Linked record | Self-referential; groups movement variants (e.g. Bench Press > Close-Grip > Incline) |
| Substitution/Scaling Links | Linked records | Scaling or substitute movements; semantically distinct from variants |

---

## APP-SPECIFIC FIELDS (NOT IN AIRTABLE)

### App 1 — Master Programming App (Supabase only)
- Format Library: wod_formats, composite_formats, composite_format_blocks
- Workout structures: workout_sections, workout_templates, template_parts, template_movements
- movement_pattern_rules: config table with spacing_days per Primary Movement Pattern for AI programming constraints
- cycle_movement_settings: per movement per cycle frequency overrides

### App 2 — Personal Training App (Supabase only)
- Sets, reps, weight, RIR per set
- Session dates and coach/client relationships
- Programmed vs completed distinction
- Estimated 1RM and volume calculations
- PR history and analytics
- Assisted movement weight direction logic
- client_movement_muscles (per-client muscle overrides)

### App 3 — Athlete Self-Tracking Widget (Supabase only, not yet built)
- Ad hoc movement logs (movement, sets, reps, RIR, notes, date)
- Rep max history per movement
- Estimated 1RM calculations
- PR flags
- WOD Library (name, description text, score type)
- WOD score log (date, score, notes)

---

## MIGRATION NOTES

- All apps preserve existing Supabase movement UUIDs as legacy_supabase_id in Airtable
- Airtable is the editorial/admin interface, not the runtime database
- All apps read from a local Supabase mirror, refreshed via Airtable webhooks
- All apps write through an edge function, never directly to Airtable
- App 2 exports first as master owner and baseline
- App 1 exports second after local normalization pass (deduplication, naming cleanup, abbreviation expansion)
- is_deprecated enforced at all times — no hard deletes ever
- Movements with multiple current patterns or equipment values require an editorial pass to select single primary before migration

---

## FLAG MUTUAL EXCLUSION RULES (enforced per-app, not in Airtable)

1. is_smash forces: time_based = true, strength_trackable = false, is_bodyweight = false
2. is_bodyweight = true (plain) forces: is_assisted = false, strength_trackable = true, is_paired = false
3. is_bodyweight + is_assisted forces: strength_trackable = true, is_paired = false
4. is_paired disabled when: is_bodyweight = true AND strength_trackable = false
5. is_assisted only valid when: is_bodyweight = true OR machine_type = pin_selector
6. machine_type = plate_loaded or free_motion_cable forces: is_assisted = false
7. No negative weights ever

---

## OWNERSHIP & WRITE RULES

- App 2 is master owner of movement data
- All apps can create and edit movements
- New movements are immediately available (provisional_status = pending_approval for admin review only)
- No hard deletes ever — use is_deprecated flag
- Naming must be canonical — duplicate detection to be enforced at write time
- Coach-only CRUD restrictions enforced per-app, not in Airtable

---

## VERSION HISTORY

v1.0 — May 23, 2026 — Initial schema locked following architecture sessions with App 1 and App 2

