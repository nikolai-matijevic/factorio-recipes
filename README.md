# Extracting the recipes

The method has been derived from this thread: https://www.reddit.com/r/factorio/comments/69h6wd/question_recipes_data/

```
/silent-command
game.player.force.enable_all_recipes()
game.player.force.enable_all_technologies()
game.player.force.research_all_technologies(1)

/silent-command
listresources = {}
for a, b in pairs(game.player.force.recipes) do
    item = "\"name\": \"" .. b.name .. "\", \"time\": " .. b.energy .. ", \"products\": {"
    for c,d in pairs (b.products) do
        if d.amount ~= nil then    
            item = item .. "\"" .. d.name .. "\":" .. d.amount .. ", "
        end
    end
    item = item .. "}, \"ingredients\": {"
    for x,y in pairs (b.ingredients) do
        item = item .. "\"" .. y.name .. "\":" .. y.amount .. ", "
    end
    item = item .. "},"
    table.insert(listresources,item) 
end
table.sort(listresources)
game.write_file("recipies.txt", table.concat(listresources, "\r\n"))
```

This will give us the following output:

```
"name": "accumulator", "time": 10, "products": {"accumulator":1, }, "ingredients": {"iron-plate":2, "battery":5, },
"name": "advanced-circuit", "time": 6, "products": {"advanced-circuit":1, }, "ingredients": {"plastic-bar":2, "copper-cable":4, "electronic-circuit":2, },
"name": "advanced-oil-processing", "time": 5, "products": {"heavy-oil":25, "light-oil":45, "petroleum-gas":55, }, "ingredients": {"water":50, "crude-oil":100, },
"name": "arithmetic-combinator", "time": 0.5, "products": {"arithmetic-combinator":1, }, "ingredients": {"copper-cable":5, "electronic-circuit":5, },
[...]
```

With a little bit of cleaning we get a valid JSON document:

```JSON
[
    {
        "name": "accumulator",
        "time": 10,
        "products": {
            "accumulator": 1
        },
        "ingredients": {
            "iron-plate": 2,
            "battery": 5
        }
    },
    {
        "name": "advanced-circuit",
        "time": 6,
        "products": {
            "advanced-circuit": 1
        },
        "ingredients": {
            "plastic-bar": 2,
            "copper-cable": 4,
            "electronic-circuit": 2
        }
    }
]
```
