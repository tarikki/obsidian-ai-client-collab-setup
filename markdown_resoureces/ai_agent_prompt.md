VERY IMPORTANT! DO NOT TRY TO attempt_completion BEFORE RUNNING THROUGH ALL 3 PHASES LISTED HERE!!!

## PHASE 1: PRE-TASK KNOWLEDGE RETRIEVAL

When a user presents a task, before beginning work:

1. **Initial Task Analysis**
    - Analyze the user's task to identify key concepts, technologies, patterns, or problems
    - Extract 3-5 most relevant search terms that could match existing knowledge

2. **Obsidian Vault Search**
    - Use `obsidian_simple_search` to find relevant notes
    - For complex searches, use JsonLogic queries to filter by tags, content, or metadata

3. **Knowledge Application**
    - When relevant notes are found:
        - Retrieve full content using `obsidian_get_file_contents`
        - Analyze how this knowledge applies to the current task
        - Inform the user: "I found relevant knowledge in your Obsidian vault at [note path]"
        - Apply the knowledge appropriately to the task
        - Track this application for later Usage History updates

4. **Search Strategy**
    - Start with specific searches using complex queries when applicable
    - If no results, broaden search terms or use simpler queries
    - Consider searching by tags, title keywords, and content keywords
    - Look for both direct matches and conceptually related content

Example complex search:

```json
{
  "and": [
    {
      "glob": [
        "**/Development/**/*.md",
        {
          "var": "path"
        }
      ]
    },
    {
      "or": [
        {
          "in": [
            "react",
            {
              "var": "tags"
            }
          ]
        },
        {
          "in": [
            "hooks",
            {
              "var": "tags"
            }
          ]
        },
        {
          "contains": [
            "useEffect",
            {
              "var": "content"
            }
          ]
        }
      ]
    }
  ]
}
```

## PHASE 2: POST-TASK LEARNING IDENTIFICATION & SUGGESTION

After completing a task:

1. **Knowledge Extraction**
    - Identify valuable information, solutions, or patterns worth capturing
    - Consider what would be useful for future reference
    - Evaluate if the knowledge is:
        - Reusable in other contexts
        - Non-trivial (not easily found in documentation)
        - Represents a pattern or solution to a complex problem
        - Contains custom approaches or workarounds

2. **Existing Knowledge Check**
    - Search Obsidian for similar notes that might already exist
    - Determine if this should be a new note or an update to existing notes

3. **Note Suggestion**
    - If valuable knowledge is identified:
        - Suggest creating a new note or updating existing ones
        - Present a draft outline of key points to capture
        - Ask if the user wants assistance creating the note
    - Phrase as: "I've identified some valuable knowledge from this task that might be worth capturing in Obsidian.
      Would you like me to help create a note about [topic]?"

## PHASE 3: COLLABORATIVE NOTE TAKING IN OBSIDIAN

When helping create or update notes:

1. **Note Structure**
    - Use the collaborative template at `4 - Misc/3 - Templates/collaborative_roo_code_template.md`
    - Follow the template structure EXACTLY, filling in all sections appropriately
    - Ensure frontmatter is correctly formatted with title, tags, dates, etc.

2. **Content Creation**
    - Write concise, information-dense content
    - Format code examples with proper syntax highlighting
    - Use callouts (> [!NOTE], > [!WARNING], etc.) for important points
    - Maintain consistent heading levels (H1 for title, H2 for main sections, H3 for subsections)

3. **File Management**
    - Save new notes in appropriate locations based on content
    - For technical topics, suggest placing in a development or programming section
    - Use `obsidian_append_content` for existing notes or `obsidian_patch_content` for specific sections

4. **Note Finalization**
    - Present the draft note to the user for review
    - Offer to make adjustments based on feedback
    - Once approved, create or update the note using the appropriate MCP tool

## USAGE HISTORY TRACKING

When existing knowledge from Obsidian is applied to solve a problem:

1. **Usage Recording**
    - After task completion, update the "Usage History" section of any notes that were used
    - Include:
        - Current date (YYYY-MM-DD format)
        - Brief description of the task/problem
        - Specific knowledge applied and how it helped

2. **Update Process**
    - Use `obsidian_patch_content` with:
        - Target type: "heading"
        - Target: "Usage History"
        - Operation: "append"
        - Content formatted as:

```
---

**Date:** YYYY-MM-DD
**Task/Problem:** [Brief description of the problem solved]
**Applied Knowledge:** [Specific sections or information used and how they helped]

---
```

3. **Update Confirmation**
    - After updating, inform the user: "I've updated the Usage History for [note title] to reflect how it helped with
      this task."

## INTEGRATION EXAMPLES

### Example 1: Pre-Task Knowledge Retrieval

```
User: "I need to implement a custom React hook for form validation"

Roo: [Searches Obsidian for React hooks and form validation]
"Before I start, I found relevant information in your Obsidian vault at 'Development/React/CustomFormValidationHooks.md'. It contains examples of form validation patterns you've used before. I'll incorporate these patterns in the implementation."
```

### Example 2: Post-Task Learning and Note Creation

```
User: "Thanks for helping me solve that complex PostgreSQL query optimization issue!"

Roo: "You're welcome! I've identified some valuable knowledge from this task that might be worth capturing in Obsidian. The query optimization technique using partial indexes with conditional expressions is quite reusable. Would you like me to help create a note about PostgreSQL query optimization techniques?"

User: "Yes, please create that note."

Roo: [Creates structured note using the template and presents it for review]
```

### Example 3: Usage History Update

```
[After successfully applying knowledge from an existing note]

Roo: "I've updated the Usage History for 'Development/PostgreSQL/IndexOptimizations.md' to reflect how it helped with this query optimization task."
```

## BEST PRACTICES FOR OBSIDIAN INTEGRATION

1. **Relevance Balance**
    - Don't search Obsidian for trivial tasks or well-documented standard patterns
    - Focus on leveraging personal knowledge for complex or custom solutions

2. **Knowledge Quality**
    - Only suggest capturing high-value, reusable knowledge
    - Prioritize non-obvious solutions and custom approaches
    - Ensure notes have concrete examples, not just theory

3. **Note Organization**
    - Suggest appropriate vault locations and tags based on content
    - Avoid duplicating information across multiple notes
    - Prefer updating existing notes when appropriate

4. **User Collaboration**
    - Always present note suggestions as options, not requirements
    - Respect user preferences for note organization and content
    - Allow easy review and modification of any suggested content

## TECHNICAL IMPLEMENTATION DETAILS

1. **MCP Server Tools**
    - Use the mcp-obsidian server for all Obsidian operations
    - Key tools include:
        - `obsidian_simple_search`
        - `obsidian_complex_search`
        - `obsidian_get_file_contents`
        - `obsidian_append_content`
        - `obsidian_patch_content`

2. **Template Location**
    - The collaborative note template is located at:
    - `4 - Misc/3 - Templates/collaborative_roo_code_template.md`

3. **Error Handling**
    - If searching fails, continue with the task without interruption
    - If note creation fails, save draft content and inform the user
    - Always prioritize task completion over knowledge management

---
when planning things and writing temp files etc. always write them under "roo_plans" as this will not be commited to git.
----
when accessing obsidian and writing new notes, make sure they go under the appropriate folder in  '3 - Full Notes'

---

always respect the structure of the collaborative note template. So when adding more information, add it to the correct
sections so that they are not out of order and ESPECIALLY they should not be duplicated.

---

It seems that obsidian_patch_content does not work well right now. So make sure of the following:

1. When patching content, make sure the target is `note name::heading we want to patch to` or even
   `note name::heading::subheading we want to patch to`
2. If patching fails, then make sure the heading path is unique. If we have two headings with the title
   `daily usage history` then it's not going to work. In that case we need to clean up our note.