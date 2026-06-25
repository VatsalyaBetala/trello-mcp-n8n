# Tool Parameter Reference

Full parameter documentation for all 18 Trello MCP tools.

---

## Boards

### `trello_get_all_boards`

Returns all Trello boards accessible to the authenticated user.

**Parameters:** none

**Returns:** `id`, `name`, `url`, `shortUrl`, `closed`

**Usage tip:** Always call this first. Board IDs returned here are required by most other tools.

---

### `trello_get_board_members`

Returns all members of a specific board.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Board_ID` | string | ✅ | Board ID from `trello_get_all_boards` |

**Returns:** `id`, `fullName`, `username`

**Usage tip:** Call before `trello_assign_member` to look up member IDs.

---

### `trello_get_board_labels`

Returns all labels defined on a board.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Board_ID` | string | ✅ | Board ID from `trello_get_all_boards` |

**Returns:** `id`, `name`, `color`

**Usage tip:** Call before `trello_assign_label` to look up label IDs.

---

## Lists

### `trello_get_lists_on_board`

Returns all open lists on a board.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Board_ID` | string | ✅ | Board ID from `trello_get_all_boards` |

**Returns:** `id`, `name`, `closed`, `pos`

---

### `trello_get_list_cards`

Returns all open cards in a specific list.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `List_ID` | string | ✅ | List ID from `trello_get_lists_on_board` |

**Returns:** `id`, `name`, `desc`, `due`, `labels`, `url`, `idList`, `idMembers`

---

### `trello_create_list`

Creates a new list on a board.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Board_ID` | string | ✅ | Board to create the list on |
| `List_Name` | string | ✅ | Name for the new list |
| `Position` | string | ❌ | `"top"`, `"bottom"`, or a positive number. Defaults to `"bottom"` |

**Returns:** Created list object with `id`, `name`, `pos`

---

### `trello_archive_list`

Archives (closes) a list. The list is hidden from the board but not permanently deleted.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `List_ID` | string | ✅ | List to archive |

> ⚠️ Confirm with the user before calling. This cannot be undone via this tool.

---

## Cards

### `trello_get_board_cards`

Returns all open cards on a board.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Board_ID` | string | ✅ | Board ID from `trello_get_all_boards` |

**Returns:** `id`, `name`, `desc`, `idList`, `due`, `labels`, `url`, `idMembers`

---

### `trello_get_card_details`

Returns full details for a single card including comments, checklists, and attachments.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Card_ID` | string | ✅ | Card ID from any card-listing tool |

**Returns:** `id`, `name`, `desc`, `due`, `dueComplete`, `idList`, `idBoard`, `labels`, `idMembers`, `url`, `shortUrl`, plus `actions` (comments), `checklists`, `attachments`, `members`

---

### `trello_search_cards`

Searches cards using Trello's native search syntax.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Query` | string | ✅ | Search query. Supports Trello operators (see below) |

**Trello search operators:**

| Operator | Example | Description |
|---|---|---|
| `is:open` | `is:open` | Only open cards |
| `is:archived` | `is:archived` | Only archived cards |
| `due:week` | `due:week` | Cards due within a week |
| `due:overdue` | `due:overdue` | Overdue cards |
| `label:` | `label:bug` | Cards with a specific label |
| `@me` | `@me` | Cards assigned to you |
| `board:` | `board:roadmap` | Limit to a specific board |
| `list:` | `list:"In Progress"` | Limit to a specific list |

**Returns:** Up to 20 matching cards with `id`, `name`, `desc`, `idList`, `due`, `labels`, `url`, `idBoard`

---

### `trello_create_card`

Creates a new card on a list.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `List_ID` | string | ✅ | List to create the card in |
| `Card_Name` | string | ✅ | Name of the card |
| `Description` | string | ❌ | Card description (supports Markdown) |
| `Due_Date` | string | ❌ | Due date in ISO-8601 format e.g. `2025-12-31T17:00:00.000Z` |
| `Member_IDs` | string | ❌ | Comma-separated member IDs to assign |
| `Label_IDs` | string | ❌ | Comma-separated label IDs to apply |

**Returns:** Created card object

---

### `trello_update_card`

Updates one or more fields on an existing card. Only fields you provide are changed.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Card_ID` | string | ✅ | Card to update |
| `New_Name` | string | ❌ | New card title |
| `New_Description` | string | ❌ | New card description |
| `New_Due` | string | ❌ | New due date in ISO-8601 format |
| `New_List_ID` | string | ❌ | Move card to this list ID |

**Note:** Omitted fields are not changed. To clear a due date, pass an empty string.

---

### `trello_archive_card`

Archives a card. The card is hidden from the board but not permanently deleted.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Card_ID` | string | ✅ | Card to archive |

> ⚠️ Confirm with the user before calling.

---

### `trello_move_card`

Moves a card to a different list, optionally on a different board.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Card_ID` | string | ✅ | Card to move |
| `Target_List_ID` | string | ✅ | Destination list ID |
| `Target_Board_ID` | string | ❌ | Destination board ID — only needed when moving across boards |

---

### `trello_assign_member`

Adds a member to a card without removing existing assignees.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Card_ID` | string | ✅ | Card to assign the member to |
| `Member_ID` | string | ✅ | Member ID from `trello_get_board_members` |

---

### `trello_assign_label`

Adds a label to a card without removing existing labels.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Card_ID` | string | ✅ | Card to assign the label to |
| `Label_ID` | string | ✅ | Label ID from `trello_get_board_labels` |

---

## Comments

### `trello_get_comments`

Returns all comments on a card in reverse chronological order.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Card_ID` | string | ✅ | Card to fetch comments from |

**Returns:** `data.text` (comment body), `date`, `memberCreator` (author)

---

### `trello_add_comment`

Adds a comment to a card.

**Parameters:**

| Name | Type | Required | Description |
|---|---|---|---|
| `Card_ID` | string | ✅ | Card to comment on |
| `Comment_Text` | string | ✅ | Comment body (plain text) |
