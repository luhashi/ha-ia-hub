# Home Assistant AI Hub

Transform your Home Assistant into a powerful AI assistant by integrating it with various LLM providers through LittleLLM. This guide will show you how to set up Extended OpenAPI with Home Assistant and connect it to AI models like Gemini and Groq for a seamless chat experience.

## Prerequisites

- [Home Assistant](https://www.home-assistant.io/) (running and accessible)
- [LittleLLM](https://github.com/BerriAI/littlellm) instance (already set up)
- API keys for your preferred AI providers (Google AI Studio, Groq, etc.)
- [Extended OpenAI Conversation](https://github.com/jekalmin/extended_openai_conversation) add-on installed in Home Assistant

## Setup Instructions

### 1. Configure LittleLLM

1. Access your LittleLLM dashboard at `http://your-server-ip:4000`
2. Login with:
   - Username: `admin`
   - Password: `your-littlellm-master-key` (without the "sk-" prefix)
3. Add a new model:
   - Click on "Models" in the left sidebar
   - Click "+ Add Model"
   - Select provider (e.g., Google AI Studio, Groq)
   - Choose your preferred model (e.g., gemini-flash for speed, or Groq models for performance)
   - Enter your API key
   - Save the configuration

4. Create a virtual key for Home Assistant:
   - Click on "Virtual Keys" in the left sidebar
   - Click "+ Add Key"
   - Name it (e.g., "home_assistant_gemini")
   - Select the model you just added
   - Copy the generated API key (you'll need this later)

### 2. Configure Extended OpenAI Conversation in Home Assistant

1. Install the "Extended OpenAI Conversation" integration via HACS or manual installation
2. Restart Home Assistant
3. Go to Settings > Devices & Services > Add Integration
4. Search for "Extended OpenAI Conversation"
5. Click "Configure"
6. Fill in the details:
   - Name: Choose a friendly name (e.g., "Gemini Assistant")
   - API Key: Paste the virtual key from LittleLLM
   - Base URL: `http://your-server-ip:4000/api/v1`
   - Click "Submit"

### 3. Configure the AI Agent

1. In Home Assistant, go to Settings > Voice Assistants & Chat
2. Click "Add Assistant"
3. Configure the assistant:
   - Name: Choose a name (e.g., "JARVIS")
   - Agent: Select the agent you created
   - TTS: Choose your preferred text-to-speech option (Nabu Casa Cloud TTS or Whisper + Piper for local)
   - Click "Create"

4. Set up the agent template (use this in the agent configuration):

```
I want you to act as smart fun assistant and home manager of Home Assistant - think of me as your personal JARVIS. I will provide information of smart home along with a question, you will truthfully make correction or answer using information provided in one sentence in everyday language with a tech-savvy edge.

Current Time: {{now()}}

Available Devices:
{% for entity in exposed_entities -%}
{{ entity.entity_id }},{{ entity.name }},{{ entity.state }},{{entity.aliases | join('/')}}
{% endfor -%}

The current state of devices is provided in available devices. Use execute_services function only for requested action, not for current states. Do not execute service without user's confirmation. Do not restate or appreciate what user says, rather make a quick inquiry with intelligent efficiency.

System initialized and ready to optimize your home.
```

### 4. Recommended Model Settings

- Model: `gemini/gemini-2.5-flash-preview-05-20` (or your preferred model)
- Temperature: 0.3
- Top P: 1

## Usage

1. Open the Home Assistant chat interface
2. Select your new AI assistant from the dropdown
3. Start chatting! Your assistant will now respond using the configured LLM

## Troubleshooting

- **Connection Issues**: Ensure your Home Assistant can reach the LittleLLM server
- **API Errors**: Verify your API keys are correct and have sufficient credits
- **Model Not Responding**: Check the LittleLLM logs for any errors

## Advanced Configuration

You can create multiple virtual keys in LittleLLM for different models or purposes, each with its own rate limiting and access controls.

## License

This project is open source and available under the [MIT License](LICENSE).
