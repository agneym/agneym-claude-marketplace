---
name: gemini-imagegen
description: Generate and edit images using Gemini API (Nano Banana Pro). Specialized for creating high-quality technical illustrations, architecture diagrams, code concept visualizations, and educational content from codebases. Supports text-to-image, image editing, multi-turn refinement, Google Search grounding for factual accuracy, and composition from multiple reference images.
---

# Gemini Image Generation (Nano Banana Pro)

Generate professional-quality images and technical illustrations using Google's **Gemini 3 Pro Image** model (aka Nano Banana Pro). The environment variable `GEMINI_API_KEY` must be set.

## Model

**gemini-3-pro-image-preview** (Nano Banana Pro)
- Resolution: Up to 4K (1K, 2K, 4K)
- Built on Gemini 3 Pro with advanced reasoning and real-world knowledge
- Best for: Professional assets, complex technical diagrams, text rendering, codebase concept illustrations
- Features: Google Search grounding, automatic "Thinking" process for refined composition

## Quick Start Scripts

CRITICAL FOR AGENTS: These are executable scripts in your PATH. All scripts now default to **gemini-3-pro-image-preview**.

### Text-to-Image
```bash
scripts/generate_image.py "A technical diagram showing microservices architecture" output.png
```

### Edit Existing Image
```bash
scripts/edit_image.py diagram.png "Add API gateway component with arrows showing data flow" output.png
```

### Multi-Turn Chat (Iterative Refinement)
```bash
scripts/multi_turn_chat.py
```

For high-resolution technical diagrams:
```bash
scripts/generate_image.py "Your prompt" output.png --size 4K --aspect 16:9
```

## Core API Pattern

All image generation uses the `generateContent` endpoint with `responseModalities: ["TEXT", "IMAGE"]`:

```python
import os
from google import genai

client = genai.Client(api_key=os.environ["GEMINI_API_KEY"])

response = client.models.generate_content(
    model="gemini-3-pro-image-preview",
    contents=["Your prompt here"],
)

for part in response.parts:
    if part.text:
        print(part.text)
    elif part.inline_data:
        image = part.as_image()
        image.save("output.png")
```

## Image Configuration Options

Control output with `image_config`:

```python
from google.genai import types

response = client.models.generate_content(
    model="gemini-3-pro-image-preview",
    contents=[prompt],
    config=types.GenerateContentConfig(
        response_modalities=['TEXT', 'IMAGE'],
        image_config=types.ImageConfig(
            aspect_ratio="16:9",  # 1:1, 2:3, 3:2, 3:4, 4:3, 4:5, 5:4, 9:16, 16:9, 21:9
            image_size="4K"       # 1K, 2K, 4K (Nano Banana Pro supports up to 4K)
        ),
    )
)
```

## Editing Images

Pass existing images with text prompts:

```python
from PIL import Image

img = Image.open("input.png")
response = client.models.generate_content(
    model="gemini-3-pro-image-preview",
    contents=["Add a sunset to this scene", img],
)
```

## Multi-Turn Refinement

Use chat for iterative editing:

```python
from google.genai import types

chat = client.chats.create(
    model="gemini-3-pro-image-preview",
    config=types.GenerateContentConfig(response_modalities=['TEXT', 'IMAGE'])
)

response = chat.send_message("Create a logo for 'Acme Corp'")
# Save first image...

response = chat.send_message("Make the text bolder and add a blue gradient")
# Save refined image...
```

## Prompting Best Practices for Nano Banana Pro

### Core Prompt Structure (Keep Under 25 Words)
Research shows prompts under 25 words achieve **30% higher accuracy** than long paragraphs. Structure prompts as:

**Subject + Adjectives + Action + Location/Context + Composition + Lighting + Style**

### Photorealistic Scenes
Include camera details: lens type, lighting, angle, mood.
> "Photorealistic close-up portrait, 85mm lens, soft golden hour light, shallow depth of field"

### Stylized Art
Specify style explicitly:
> "Kawaii-style sticker of a happy red panda, bold outlines, cel-shading, white background"

### Text in Images
Nano Banana Pro excels at text rendering. Be explicit about font style and placement:
> "Logo with text 'Daily Grind' in clean sans-serif, black and white, coffee bean motif"

### Product Mockups
Describe lighting setup and surface:
> "Studio-lit product photo on polished concrete, three-point softbox setup, 45-degree angle"

## Codebase Concept Illustrations

Specialized prompts for creating educational illustrations from code concepts.

**CRITICAL GUIDELINES FOR LLM AGENTS:**
1. **Never be vague**: Specify exact positions (top, bottom, left, right, center)
2. **Explicit relationships**: State every connection with labels (e.g., "Arrow from A to B labeled 'HTTP GET'")
3. **Complete data flows**: Include all steps, methods, parameters, and return types
4. **Specific colors**: Assign colors to different elements with clear meaning
5. **No inference**: The image model cannot see the codebase - provide ALL details explicitly
6. **Cardinality/direction**: Always specify relationship directions and multiplicities

These templates guide you to construct prompts from codebase analysis. Replace [PLACEHOLDERS] with specific values extracted from the code.

### Architecture Diagrams
**Template:**
```
"Technical architecture diagram: [COMPONENT_1] at top, [COMPONENT_2] on left, [COMPONENT_3] on right.
Arrow from [COMPONENT_1] to [COMPONENT_2] labeled '[CONNECTION_TYPE]'.
Arrow from [COMPONENT_2] to [COMPONENT_3] labeled '[DATA_FLOW]'.
[Repeat for all connections with explicit labels].
Clean boxes, directional arrows with labels, white background, technical style."
```

**Example (Explicit):**
> "Technical architecture diagram: API Gateway at top center, Auth Service on left middle, User Service on right middle, Database at bottom center, Redis Cache on bottom left. Arrow from API Gateway to Auth Service labeled 'POST /auth/login JWT validation'. Arrow from API Gateway to User Service labeled 'GET /users authenticated requests'. Arrow from Auth Service to Database labeled 'SQL queries users table'. Arrow from User Service to Database labeled 'SQL CRUD operations'. Arrow from User Service to Redis Cache labeled 'Cache GET/SET user sessions'. Clean labeled boxes, directional arrows with protocol labels, white background."

### Data Flow Diagrams
**Template:**
```
"Data flow diagram for [PROCESS]: Step 1: [ENTITY_A] at left. Step 2: Arrow to [ENTITY_B] labeled '[ACTION/DATA]'.
Step 3: Arrow back to [ENTITY_A] labeled '[RESPONSE]'. [Continue for all steps].
Number each step, label all arrows with HTTP methods/data types, color-code by data type:
[TYPE_1] in blue, [TYPE_2] in green. Technical style, 16:9 aspect ratio."
```

**Example (Explicit):**
> "OAuth 2.0 flow diagram: Step 1: Client (blue box) at left. Step 2: Arrow to Authorization Server (green box) at top, labeled 'GET /authorize?client_id=123'. Step 3: Arrow back to Client labeled '302 redirect with auth code'. Step 4: Arrow from Client to Authorization Server labeled 'POST /token with auth code'. Step 5: Arrow back to Client labeled 'access_token + refresh_token JSON'. Step 6: Arrow from Client to Resource Server (orange box) at right labeled 'GET /api/data with Bearer token'. Step 7: Arrow back to Client labeled 'JSON response data'. Number all steps 1-7, color-code: requests in blue, responses in green, tokens in orange."

### Algorithm Visualizations
**Template:**
```
"[ALGORITHM_NAME] visualization: Stage 1: [DATA_STRUCTURE_STATE_1] showing [VALUES].
Stage 2: [TRANSFORMATION] resulting in [DATA_STRUCTURE_STATE_2].
Stage 3: [NEXT_STEP] with values [SPECIFIC_VALUES]. [Continue for all stages].
Label time complexity '[COMPLEXITY]' at bottom. Color-code: [CONDITION_1] in red, [CONDITION_2] in green.
Arrows show progression between stages. Educational infographic style."
```

**Example (Explicit):**
> "Merge sort visualization: Stage 1: Initial array [38, 27, 43, 3, 9, 82, 10] in gray box at top. Stage 2: Split into [38, 27, 43, 3] and [9, 82, 10] with arrow down labeled 'divide'. Stage 3: Further split into [38, 27], [43, 3], [9, 82], [10] with arrows labeled 'recursive divide'. Stage 4: Merge [27, 38] (sorted in green), [3, 43] (sorted in green), [9, 82] (sorted in green), [10] shown. Stage 5: Merge [3, 27, 38, 43] and [9, 10, 82] with comparison arrows. Stage 6: Final sorted array [3, 9, 10, 27, 38, 43, 82] in blue box at bottom. Label 'Time: O(n log n), Space: O(n)' at bottom. Unsorted in gray, sorted in green, final in blue."

### Class/Component Relationships
**Template:**
```
"UML class diagram: [CLASS_1] box with attributes [ATTR_LIST] and methods [METHOD_LIST].
[CLASS_2] box with [ATTRS/METHODS]. [Specify relationship]: [CLASS_1] [RELATIONSHIP_TYPE] [CLASS_2].
Example: '[CLASS_A] inherits from [CLASS_B]' shown with hollow arrow from A to B.
'[CLASS_C] has-a [CLASS_D]' shown with diamond and line. '[CLASS_E] uses [CLASS_F]' shown with dashed arrow.
Include cardinality: '1' to '* (many)' labels on lines. Clean technical UML style."
```

**Example (Explicit):**
> "UML class diagram: Customer box at top with attributes '-id: int, -name: string, -email: string' and methods '+placeOrder(), +getOrders()'. Order box at middle left with '-orderId: int, -date: Date, -status: string' and '+calculateTotal(), +cancel()'. Product box at middle right with '-productId: int, -name: string, -price: float' and '+getDetails()'. Payment box at bottom with '-paymentId: int, -amount: float' and '+processPayment()'. Customer to Order: solid line with hollow diamond at Customer end, labeled '1' to '* (many)', showing composition 'Customer has many Orders'. Order to Product: solid line labeled 'M to N', showing 'Order contains many Products'. Order to Payment: solid line with diamond, labeled '1 to 1', showing 'Order has one Payment'."

### API Endpoint Visualization
**Template:**
```
"REST API diagram for [SERVICE_NAME]: [ENDPOINT_1]: [METHOD] [PATH] with request body {[FIELDS]} returns {[RESPONSE_FIELDS]}.
[ENDPOINT_2]: [METHOD] [PATH] with params [PARAMS] returns [RESPONSE]. [List all endpoints with full details].
Color-code: GET in blue, POST in green, PUT in yellow, DELETE in red.
Show example payloads. Technical documentation style."
```

**Example (Explicit):**
> "REST API diagram for User Service: Endpoint 1: GET /api/users/:id with path param 'id: integer' returns {id, name, email, createdAt} in JSON, blue box. Endpoint 2: POST /api/users with request body {name: string, email: string, password: string} returns {id, name, email} 201 Created, green box. Endpoint 3: PUT /api/users/:id with path param 'id' and body {name?, email?} returns {id, name, email} 200 OK, yellow box. Endpoint 4: DELETE /api/users/:id with path param 'id' returns 204 No Content, red box. Endpoint 5: GET /api/users with query params 'limit: int, offset: int' returns {users: [], total: int}, blue box. Show full JSON examples for each. Color-code by HTTP method."

### Code Execution Flow
**Template:**
```
"Flowchart for [FUNCTION_NAME]: Start at top. Step 1: [ACTION] in rectangle. Step 2: Decision diamond '[CONDITION]'
with YES arrow to [NEXT_STEP] and NO arrow to [ALTERNATIVE]. [Continue for all steps].
Terminal error cases: [ERROR_1] in red rounded rectangle, [ERROR_2] in red.
Success end: [SUCCESS_STATE] in green rounded rectangle. Label all arrows with conditions.
Standard flowchart symbols."
```

**Example (Explicit):**
> "Flowchart for authenticateUser(): Start. Step 1: 'Receive username and password' in blue rectangle. Step 2: Decision diamond 'username exists in DB?' with NO arrow to red rounded box 'Return 404 User Not Found' (terminal), YES arrow to Step 3. Step 3: 'Retrieve user record and password hash' in blue rectangle. Step 4: Decision diamond 'bcrypt.compare(password, hash) matches?' with NO arrow to red box 'Return 401 Invalid Credentials' (terminal), YES arrow to Step 5. Step 5: Decision diamond 'user.isActive === true?' with NO arrow to red box 'Return 403 Account Disabled' (terminal), YES arrow to Step 6. Step 6: 'Generate JWT token with user.id' in blue rectangle. Step 7: 'Create session record in Redis with 24h TTL' in blue rectangle. Step 8: 'Return 200 with {token, user}' in green rounded box (terminal success). All error paths in red, success path in green, decisions in yellow diamonds."

### Database Schema Visualization
**Template:**
```
"Database schema for [SYSTEM]: [TABLE_1] table with columns: [COL_1] [TYPE] PRIMARY KEY, [COL_2] [TYPE] NOT NULL, [etc].
[TABLE_2] table with [COLUMNS]. Foreign key: [TABLE_2].[FK_COLUMN] references [TABLE_1].[PK_COLUMN]
shown with line from [TABLE_2] to [TABLE_1] labeled '[RELATIONSHIP]'. [Repeat for all tables and relationships].
Show cardinality: 1:1, 1:N, N:M on relationship lines. Technical database diagram style."
```

**Example (Explicit):**
> "E-commerce database schema: Users table with columns 'user_id INTEGER PRIMARY KEY, username VARCHAR(50) UNIQUE NOT NULL, email VARCHAR(100) UNIQUE NOT NULL, created_at TIMESTAMP'. Orders table with 'order_id INTEGER PRIMARY KEY, user_id INTEGER NOT NULL, total_amount DECIMAL(10,2), status VARCHAR(20), created_at TIMESTAMP'. Products table with 'product_id INTEGER PRIMARY KEY, name VARCHAR(100) NOT NULL, price DECIMAL(10,2) NOT NULL, stock INTEGER'. OrderItems table with 'order_item_id INTEGER PRIMARY KEY, order_id INTEGER NOT NULL, product_id INTEGER NOT NULL, quantity INTEGER, price DECIMAL(10,2)'. Foreign keys: Orders.user_id → Users.user_id shown with line labeled '1 User to N Orders'. OrderItems.order_id → Orders.order_id labeled '1 Order to N OrderItems'. OrderItems.product_id → Products.product_id labeled 'N OrderItems to 1 Product'. Show PK with key icon, FK with arrow."

### Design Pattern Illustration
**Template:**
```
"[PATTERN_NAME] design pattern: [COMPONENT_1] class with methods [METHODS] at [POSITION].
[COMPONENT_2] interface with [INTERFACE_METHODS]. [COMPONENT_3] implements [INTERFACE] shown with dashed arrow.
Interaction: [STEP_1]: [COMPONENT_A] calls [METHOD] on [COMPONENT_B]. [STEP_2]: [RESULT].
Show sequence with numbered arrows. Include before/after: Before section showing [PROBLEM],
After section showing [SOLUTION] with pattern applied. Educational style with clear labels."
```

**Example (Explicit):**
> "Observer pattern: Top: Subject class with methods 'attach(observer), detach(observer), notify()' and state 'observers: List'. Middle: Observer interface with 'update(data)' method shown in dashed box. Bottom: Three concrete classes 'EmailObserver, SMSObserver, LogObserver' each implementing Observer interface shown with dashed arrows up to interface. Interaction sequence: Step 1: Subject.setState(newValue) arrow to Subject's internal state. Step 2: Subject.notify() arrow loops through observers list. Step 3: For each observer, Subject calls observer.update(data) shown with arrows from Subject to each concrete observer. Step 4: Each observer receives data and performs action shown in their boxes. Before section (left): Tightly coupled code with if-else chains checking notification types. After section (right): Clean Subject-Observer structure with loose coupling. Label 'Behavioral Pattern' at top."

## Advanced Features

### Google Search Grounding
Generate images based on real-time data:

```python
response = client.models.generate_content(
    model="gemini-3-pro-image-preview",
    contents=["Visualize today's weather in Tokyo as an infographic"],
    config=types.GenerateContentConfig(
        response_modalities=['TEXT', 'IMAGE'],
        tools=[{"google_search": {}}]
    )
)
```

### Multiple Reference Images (Up to 14)
Combine elements from multiple sources:

```python
response = client.models.generate_content(
    model="gemini-3-pro-image-preview",
    contents=[
        "Create a group photo of these people in an office",
        Image.open("person1.png"),
        Image.open("person2.png"),
        Image.open("person3.png"),
    ],
)
```

## REST API (curl)

```bash
curl -s -X POST \
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [{"parts": [{"text": "Technical diagram showing RESTful API architecture"}]}]
  }' | jq -r '.candidates[0].content.parts[] | select(.inlineData) | .inlineData.data' | base64 --decode > output.png
```

## Notes

- All generated images include SynthID watermarks
- Image-only mode (`responseModalities: ["IMAGE"]`) won't work with Google Search grounding
- For editing, describe changes conversationally—the model understands semantic masking

## Educational Philosophy

**Remember: Your goal as a teacher is to create the best visualization possible for the student to understand.**

The diagram does NOT have to be a traditional technical diagram. Consider alternative formats based on what will help the learner grasp the concept:

- **Comics/Sequential Art**: Show a function's execution as a comic strip with characters representing data
- **Metaphors**: Visualize a database transaction as a restaurant order system
- **Analogies**: Represent a cache as a kitchen pantry with frequently-used ingredients
- **Stories**: Illustrate an algorithm as a journey with characters making decisions
- **Real-world Scenarios**: Show API authentication as a bouncer checking IDs at a club
- **Visual Metaphors**: Depict memory management as organizing a bookshelf

**Examples of creative visualizations:**

> "Comic strip showing JWT authentication: Panel 1: User (detective) shows credentials at API Gateway (security desk). Panel 2: Gateway calls Auth Service (background check office) to verify. Panel 3: Auth Service returns golden badge (JWT token). Panel 4: User shows badge to access protected Resource Server (VIP room). Color: lighthearted cartoon style."

> "Restaurant kitchen metaphor for caching: Chef (CPU) at center. Pantry shelves (Cache) on left with frequently-used spices labeled 'hot data, TTL: 1 hour'. Large storage room (Database) in back with all ingredients. Arrow from Chef to Pantry labeled 'Quick access, O(1)'. Arrow from Chef to Storage labeled 'Slow access, disk I/O'. Show Chef checking Pantry first (cache hit in green), then Storage (cache miss in red)."

> "Garden ecosystem showing Observer pattern: Gardener (Subject) at center with watering can. Three plants (Observers): Rose, Cactus, Fern. When Gardener waters, all three plants receive water but react differently - Rose blooms, Cactus stores, Fern grows. Show notification 'water event' spreading from Gardener to all plants. Illustrative, nature-themed style."

The key is matching the visualization style to what will create the strongest understanding for the learner. Technical accuracy is important, but comprehension is paramount.
