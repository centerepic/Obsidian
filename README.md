# Obsidian UI Library
**Documentation:** https://docs.mspaint.cc/obsidian

**Source code to documentation site:** https://github.com/mspaint-cc/docs.mspaint.cc/tree/main/content/obsidian

## Drag Lists

Use `Groupbox:AddDragList` when you need a sortable, vertical list that supports drag-and-drop reordering. The control behaves similarly to a dropdown's `Values` array, but lets users rearrange entries directly.

```luau
local DragList = Groupbox:AddDragList("MyDragList", {
	Text = "A draggable list",
	Values = { "elem1", "elem2", "elem3", "elem4" },
	DisabledValues = { "elem4" }, -- Optional: locked entries that cannot be dragged

	-- Optional validation callback; return false to cancel a requested move
	CanMove = function(value, fromIndex, toIndex, previewOrder)
		if value == "elem1" and toIndex == 1 then
			return false
		end
		return true
	end,

	Callback = function(order)
		print("Current order", table.concat(order, ", "))
	end,
})

-- The current order is exposed through Options, just like other controls
print(Options.MyDragList.Value[1])
```

The callback (and `:OnChanged` handler) receives the latest ordered array. You can also fetch a copy at any time with `Options.MyDragList:GetValue()`. Programmatic changes are supported via `SetValue`, `SetValues`, `AddValues`, and the same disabled-value helpers used by dropdowns.

Drag lists reuse the dropdown row styling, including hover highlights, so they blend in with the rest of the library. The drag interaction is animated and works with both mouse and touch input—on mobile, just tap and hold an entry before moving it to a new slot.
