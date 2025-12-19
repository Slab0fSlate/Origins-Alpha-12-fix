# Origins-Alpha-12-fix
A cobbled together workaround for a bug in 1.13 alpha.12 of the Origins mod, works on the base origins only.

-----

in alpha.12, theres an issue where all of an origins attribute modifiers stop working after the player dies. so things like merlings swim speed, anyone meant to have less health, avians extra speed, it all just stops working.

this can be worked around by **revoking and re-granting those modifiers on respawn.** so dont worry, you can do this to your other origins if you understand how to... ive just done the more finnicky job of doing that fix on the base origins.

---

**details:**
does replace the base origins with (hopefully) identical copies.  
but Doesnt replace the origins layer or human, so hopefully this meddles with as little as possible.

**note:**
i realise i coulda probably done this a Much neater and more effective way, and ill be honest, might fix that eventually, but for now i got a bunch more origins to fix for my server, and so long as its working i dont really care, lol


# example for doing this to your datapack origins
## 1. take all your attribute modifiers and put them under one multi-power  
just about anything that says attribute or modifier in it? probably what you're after.  
heres what that looks like for merling:
```
{
    "type": "origins:multiple",
    "underwater": {
        "condition": {
            "type": "origins:all_of",
            "conditions": [
                {
                    "type": "origins:submerged_in",
                    "fluid": "minecraft:water"
                },
                {
                    "type": "origins:enchantment",
                    "enchantment": "minecraft:aqua_affinity",
                    "comparison": "==",
                    "compare_to": 0
                }
            ]
        },
        "type": "origins:modify_break_speed",
        "modifier": {
            "amount": 4,
            "operation": "multiply_total_multiplicative"
        }
    },
    "ungrounded": {
        "condition": {
            "type": "origins:all_of",
            "conditions": [
                {
                    "type": "origins:fluid_height",
                    "fluid": "minecraft:water",
                    "comparison": ">",
                    "compare_to": 0
                },
                {
                    "type": "origins:on_block",
                    "inverted": true
                }
            ]
        },
        "type": "origins:modify_break_speed",
        "modifier": {
            "amount": 4,
            "operation": "multiply_total_multiplicative"
        }
    },
    "swimspeed": {
        "type": "origins:attribute",
        "modifier": {
            "id": "*:*",
            "attribute": "additionalentityattributes:generic.water_speed",
            "amount": 1.5,
            "operation": "add_multiplied_base"
        }
    }
}
```

## 2. put this power in somewhere, and adjust to your origin
change the "power" field to be the power with all your modifiers in it  
and change the "source" to be the name of your origin

```
{
    "type": "origins:action_on_callback",
    "execute_chosen_when_orb": true,
    "entity_action_respawned": {
        "type": "origins:and",
        "actions": [
            {
                "type": "origins:revoke_power",
                "power": "fix:mer/attribs",
                "source": "fix:merling"
            },
            {
                "type": "origins:grant_power",
                "power": "fix:mer/attribs",
                "source": "fix:merling"
            }
        ]
    }
}
```

> bonus tips for version conversion: pack format is 48, "value" is now usually "amount" the "icon": "item" thing is now "icon": "id", all operation types have changed, dont forget to use your mc logs to diagnose things, make sure to read through the origins 1.13 changelogs, and also The Origin Creator still works fine for the most part, you'll just have to update the smaller changes after exporting
   
