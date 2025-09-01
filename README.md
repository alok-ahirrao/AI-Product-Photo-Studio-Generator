# AI Product Photo Studio ✨

An advanced, web-based tool that leverages the Google Gemini API to transform standard product images into premium, advertisement-ready visuals. This application provides a rich user interface to control the AI's creative process, enabling users to generate professional-quality product photos in seconds.


### GIF Demo
![Demo GIF](output/output.gif)


## 🌐 Live Demo

[![AI Product Photo Studio Demo](https://img.youtube.com/vi/TFrjMJY9JtM/maxresdefault.jpg)](https://youtu.be/TFrjMJY9JtM)

Check out the GitHub repository: [AI-Product-Photo-Studio](https://github.com/alok-ahirrao/AI-Product-Photo-Studio)
## 📜 Table of Contents

- [Live Demo](#-live-demo)
- [Introduction](#-introduction)
- [Key Features](#-key-features)
- [System Architecture](#-system-architecture)
- [How It Works](#-how-it-works)
- [🤖 Prompt Engineering Deep Dive](#-prompt-engineering-deep-dive)
- [Core Technologies](#-core-technologies)
- [Setup & Configuration](#-setup--configuration)
- [Usage Guide](#-usage-guide)
- [Future Roadmap](#-future-roadmap)
- [Contributing](#-contributing)
- [License](#-license)


## 🌟 Introduction

The AI Product Photo Studio solves a common problem for e-commerce stores, marketers, and designers: the need for high-quality, consistent, and creative product photography without the expensive overhead of a physical studio. By providing a set of intuitive controls, this tool empowers users to act as art directors, guiding a powerful AI model to produce stunning visuals that can elevate a brand's presence across any digital channel.

This application is built as a completely client-side React application, communicating directly with the Google Gemini API to perform all image generation tasks.

## 🚀 Key Features

-   **Multi-Modal Image Generation**: Utilizes the `gemini-2.5-flash-image-preview` model to understand and process both image and text inputs simultaneously.
-   **Product Image Upload**: Upload a product image (`.png` or `.jpg`) as the base for generation.
-   **Advanced Background Controls**:
    -   **Auto-generate**: Let the AI create a complementary background based on product type and selected style.
    -   **Manual Description**: Provide a detailed text prompt for the exact background you want.
    -   **Upload Image**: Upload your own background image for the AI to seamlessly composite your product into a new scene.
-   **Precise Aspect Ratio Control**: Choose from standard aspect ratios like Landscape (4:3), Square (1:1), Portrait (3:4), Widescreen (16:9), and Story (9:16) to fit any marketing channel.
-   **Rich Artistic Direction**:
    -   **Styles**: Select from a range of professional styles like "Premium & Elegant," "Clean & Minimal," or "Luxury & Glamorous."
    -   **Background Themes**: Place your product in creative environments like "Ocean & Water 🌊," "Forest & Nature 🌳," or "Luxe Silk & Satin 💎."
-   **Fine-Tuned Enhancements**: Toggle specific effects like realistic reflections, lighting boosts, premium glows, and the auto-addition of context-aware elements (e.g., fruit slices for a beverage).
-   **Client-Side Image Processing**: Pre-processes images in the browser to match the selected aspect ratio before sending them to the API, ensuring dimensionally accurate results.
-   **Intuitive UI**: Features a clean, modern interface with image previews, loading states, and a comparison view to easily see the AI's transformation.

## 🏗️ System Architecture

This is a **client-side-only application**. There is no backend server. All logic, state management, and API communication happen directly within the user's browser.

```
+--------------------------------+      +--------------------------------+
|      User's Web Browser        |      |       Google Cloud AI          |
|                                |      |                                |
|  +--------------------------+  |      |  +--------------------------+  |
|  |   React & TypeScript UI  |  |      |  |                          |  |
|  | (index.tsx)              |  |      |  |    Gemini API Endpoint   |  |
|  | - State Management (useState) |      |  | (gemini-2.5-flash-image-preview)|  |
|  | - Component Logic        |  |      |  |                          |  |
|  | - Image Pre-processing   |  |      |  +--------------------------+  |
|  +--------------------------+  |      |                                |
|              |                 |      +--------------------------------+
|              | (User Actions)  |
|              |                 |
|  +--------------------------+  |
|  |  Prompt Construction Logic |  |
|  | - Assembles "Master Prompt"|  |
|  +--------------------------+  |
|              |                 |
|              | HTTPS Request   |
|              +---------------------->
|                                |
|  <------------------------------------+
|         (Generated Image)      |
|                                |
+--------------------------------+
```

## ⚙️ How It Works

The application follows a clear, multi-step process from user input to final image display:

1.  **User Input**: The user uploads a product image and selects various creative options (style, theme, enhancements, etc.) from the control panel.
2.  **Client-Side Pre-processing**: If an aspect ratio other than the original is selected, the product image is drawn onto a new HTML `<canvas>` element in the browser. This canvas has the target aspect ratio, and the image is centered within it. The canvas is then converted back to a Base64 string.
3.  **Prompt Construction**: A detailed, "expert-level" prompt is dynamically assembled in TypeScript based on all of the user's choices. This is the core intellectual property of the application (see [Prompt Engineering Deep Dive](#-prompt-engineering-deep-dive)).
4.  **API Call**: The pre-processed image(s) (product and optional background) and the master prompt are packaged into a request object and sent directly to the Google Gemini API using the `@google/genai` SDK.
5.  **Image Display**: The API returns the newly generated image as a Base64 string. The React app updates its state, and the final image is rendered in the display panel.

## 🤖 Prompt Engineering Deep Dive

The quality of the output is entirely dependent on the quality of the prompt. This app doesn't just send a few keywords; it constructs a detailed, multi-part "master prompt" that acts as a creative brief for the AI.

The prompt is built by concatenating several structured blocks:

1.  **Core Instruction**: A preamble that sets the AI's role and primary goal.
    > *"As an expert AI product photographer, your task is to generate a premium, advertisement-ready image..."*
2.  **Style Direction**: A detailed paragraph describing the chosen aesthetic.
    > *e.g., for "Premium & Elegant"*: `"Aim for a 'quiet luxury' aesthetic. The composition must be meticulous... lighting should be soft and diffused..."*
3.  **Environmental Theme**: If a theme is chosen, a rich description of the environment is added.
    > *e.g., for "Ocean & Water"*: `"Place the product in a dynamic aquatic environment. Use elements like crystal-clear water, energetic splashes..."*
4.  **Background Details**: This block changes based on the user's background mode selection.
    -   **Auto**: Instructs the AI to generate a background based on the product type.
    -   **Manual**: Injects the user's exact text description.
    -   **Upload**: Provides explicit instructions to composite the product onto the uploaded background, matching lighting and perspective.
5.  **Fine-Tune Enhancements**: A bulleted list of specific, actionable instructions is created from the selected checkboxes.
    > *"- Apply a professional lighting boost with clear highlights..."*
    > *"- Add realistic, soft reflections and shadows..."*
6.  **Final Check**: A concluding instruction to reinforce the main objective.
    > *"The final image must look like a high-end photoshoot. The product is the absolute focus."*

This structured approach transforms simple UI selections into a sophisticated set of commands, resulting in significantly higher-quality and more consistent outputs.

## 💻 Core Technologies

-   **Frontend**: [React](https://react.dev/) (v19) with TypeScript
-   **AI Model**: Google Gemini (`gemini-2.5-flash-image-preview`)
-   **SDK**: [`@google/genai`](https://www.npmjs.com/package/@google/genai)
-   **Styling**: Inline CSS-in-JS and a dynamically injected stylesheet for global styles and animations.
-   **Dependencies**: Loaded via CDN using an `importmap` in `index.html`, requiring no local `node_modules` folder or bundler.

## 🔧 Setup & Configuration

This project is designed to be deployed on a static web host or run in a development environment that can provide environment variables.

### API Key

The application requires a Google Gemini API key to function.

1.  Obtain an API key from [Google AI Studio](https://aistudio.google.com/).
2.  The application expects this key to be available as an environment variable named `API_KEY`.
3.  When deploying to a hosting provider (like Vercel, Netlify, or a custom server), set `API_KEY` as an environment variable in the platform's settings. The application code will automatically access it via `process.env.API_KEY`.

**Note:** Do not hardcode your API key directly into the source code.

## 📖 Usage Guide

1.  **Upload Product**: Click "Choose File" under **1. Upload Product** to select your image. A preview will appear.
2.  **Describe Product**: In section **2. Product Type**, briefly describe your product (e.g., "Bottle of red wine," "White ceramic mug"). This helps the AI add relevant elements when "Auto Add Elements" is enabled.
3.  **Set Aspect Ratio**: Choose the desired output dimensions in section **3. Aspect Ratio**.
4.  **Define Background**: In section **4. Background**, choose your preferred method:
    -   **Auto**: Let the AI decide the best background. This is a great starting point.
    -   **Manual**: Write a specific description (e.g., "On a dark oak table next to a steaming cup of coffee").
    -   **Upload**: Select a custom image to use as the background.
5.  **Choose Style & Theme**: Select the overall aesthetic in section **5** and an optional environmental theme in section **6**.
6.  **Add Enhancements**: Check the boxes in section **7. Enhancements** to apply final touches.
7.  **Generate**: Click the **"Generate Image 🪄"** button and wait for the magic.
8.  **Review & Download**: The generated image will appear. Use the "Compare Original" button to see a side-by-side view and "Download" to save your new image. Click "Regenerate" to try again with the same settings.

## 🗺️ Future Roadmap

-   [ ] **Batch Processing**: Upload multiple products and apply the same settings to all of them.
-   [ ] **Negative Prompts**: Add a field to specify what *not* to include in the image.
-   [ ] **Inpainting/Outpainting**: Allow users to select an area of the image to modify or expand.
-   [ ] **History Panel**: Keep a history of generated images and their settings for easy recall.
-   [ ] **More Styles & Themes**: Continuously expand the library of available creative options.

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/your-username/your-repo/issues).

## 📜 License

Copyright © 2025, Alok Ahirrao

Licensed under the **Creative Commons Attribution-NonCommercial 4.0 International License**. You may use or modify this project for personal or educational purposes only. Commercial usage requires explicit permission.

For inquiries, please contact [alokahirrao.ai@gmail.com](mailto:alokahirrao.ai@gmail.com).

---

🌟 **Happy Coding!** 🌟
