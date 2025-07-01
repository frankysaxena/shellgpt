<script setup>
import { ref, nextTick } from 'vue'

const messages = ref([])
const newMessage = ref('')
const showWelcome = ref(true)
const isLoading = ref(false)

const prompts = [
  {
    id: 'local-offers',
    icon: '☕',
    title: 'Promote Local Offers',
    description: 'Create campaigns for your coffee deals, fuel discounts, convenience store specials, and seasonal promotions.',
    color: '#FECB00',
    keywords: ['local offers', 'coffee promotion', 'fuel discount', 'store specials', 'seasonal deals']
  },
  {
    id: 'customer-acquisition',
    icon: '🎯',
    title: 'Customer Acquisition',
    description: 'Drive new customers to your station with targeted campaigns for car wash services, loyalty programs, and grand opening events.',
    color: '#D52B1E',
    keywords: ['new customers', 'car wash', 'loyalty program', 'grand opening', 'station services']
  },
  {
    id: 'seasonal-campaigns',
    icon: '📅',
    title: 'Seasonal Campaigns',
    description: 'Leverage seasonal opportunities with holiday promotions, summer travel deals, winter prep services, and back-to-school offers.',
    color: '#D52B1E',
    keywords: ['seasonal', 'holiday deals', 'travel promotions', 'winter services', 'school season']
  }
]

const responses = {
  'local-offers': {
    title: '☕ Local Offer Campaign Strategy',
    content: `Here's your personalized campaign strategy for promoting local offers at your Shell station:

**🎯 Campaign Focus: Coffee & Convenience**
• **Offer**: "Buy 2 Coffees, Get 1 Free" morning rush special
• **Target Audience**: Morning commuters and regular customers
• **Timing**: 6:00 AM - 10:00 AM weekdays
• **Duration**: 2-week promotional period

**📱 Multi-Channel Approach:**
• **SMS**: "Start your morning right! ☕ Buy 2 coffees, get 1 FREE at Shell Station [Your Location]. Valid weekdays 6-10 AM. Show this text!"
• **Email**: Professional newsletter featuring the coffee deal + fuel points bonus
• **Mobile App Push**: Location-based notifications when customers are within 2 miles
• **In-Store**: Counter displays and pump-topper advertisements

**💡 Cross-Selling Opportunities:**
• Bundle coffee with breakfast items from your convenience store
• Promote fuel rewards program signup with coffee purchase
• Highlight car wash discounts for morning customers

**📊 Success Metrics:**
• Track coffee sales increase during promotional hours
• Monitor new customer acquisitions through offer redemption
• Measure fuel rewards program signups from coffee customers

**🚀 Next Steps:**
1. Customize SMS and email templates with your station details
2. Set up location-based targeting in the MORE! Connected platform
3. Order promotional materials for in-store displays
4. Schedule campaign launch for Monday morning rush`
  },
  'customer-acquisition': {
    title: '🎯 Customer Acquisition Campaign',
    content: `Your targeted customer acquisition strategy for driving new business to your Shell station:

**🚗 New Customer Welcome Campaign**
• **Primary Offer**: "New Customer Special - 10¢ off per gallon + Free Car Wash"
• **Target**: Households within 5-mile radius who haven't visited in 90+ days
• **Acquisition Goal**: 50 new regular customers per month

**📍 Location-Based Targeting:**
• **Geofencing**: Target competitors' stations with better offers
• **New Resident Welcome**: Reach households that recently moved to your area
• **Workplace Targeting**: Office complexes and business parks nearby
• **School Zone Parents**: Morning drop-off and afternoon pickup times

**🎁 Incentive Structure:**
• **First Visit**: 10¢ fuel discount + free premium car wash
• **Second Visit**: 5¢ fuel discount + convenience store coupon
• **Third Visit**: Shell Rewards enrollment bonus
• **Loyalty Hook**: "Complete 5 visits, get $10 Shell card"

**📢 Communication Strategy:**
• **Direct Mail**: Targeted postcards to new residents and lapsed customers
• **Digital Ads**: Facebook and Google ads with location-based targeting
• **Referral Program**: Existing customers get rewards for bringing friends
• **Community Partnerships**: Local business employee discounts

**📈 Conversion Tracking:**
• New customer identification through rewards program signup
• Visit frequency tracking for retention analysis
• Average spend per new customer acquisition
• Cost per acquisition vs. customer lifetime value

**⚡ Quick Launch Checklist:**
1. Upload your customer database to identify lapsed visitors
2. Set geofencing parameters around competitor locations
3. Create welcome packet materials for new customers
4. Train staff on new customer identification and enrollment process`
  },
  'community-events': {
    title: '🏪 Community Engagement Strategy',
    content: `Build stronger community connections and drive local foot traffic with these engagement initiatives:

**🎪 Community Event Calendar**
• **Monthly Car Care Clinic**: Free tire pressure checks and fluid top-offs
• **Quarterly Charity Fundraiser**: Partner with local schools and nonprofits
• **Seasonal Community Days**: Family-friendly events with food trucks and activities
• **Local Sports Team Sponsorship**: Support youth leagues and high school teams

**🤝 Strategic Partnerships:**
• **Local Businesses**: Cross-promotional deals with nearby restaurants and shops
• **School Partnerships**: Fundraising programs and safe driving education
• **Emergency Services**: First responder appreciation events and discounts
• **Community Organizations**: Sponsor local club meetings and charity events

**📅 Event Promotion Strategy:**
• **Community Boards**: Physical and digital bulletin board presence
• **Local Facebook Groups**: Engage in neighborhood social media groups
• **School Newsletters**: Partner with PTAs for event announcements
• **Radio Sponsorships**: Local radio station community calendar features

**🎁 Community-Focused Offers:**
• **Teacher Discounts**: Special pricing during back-to-school season
• **First Responder Perks**: Ongoing fuel and service discounts
• **Senior Hours**: Dedicated shopping times with assistance
• **Youth Sports Team Fuel**: Discounted rates for team travel

**📊 Community Impact Metrics:**
• Local event attendance and participation rates
• Community partnership referral tracking
• Social media engagement from local customers
• Repeat visit rates from event participants

**🌟 Reputation Building:**
• Sponsor local Little League teams with station branding
• Participate in town festivals and community fairs
• Host monthly "Coffee with the Owner" community meetings
• Support local food bank with customer donation matching

**🚀 Implementation Timeline:**
Week 1: Identify local organizations and potential partnerships
Week 2: Reach out to community leaders and event organizers  
Week 3: Plan first community event and promotional materials
Week 4: Launch first partnership and promote upcoming events`
  },
  'seasonal-campaigns': {
    title: '📅 Seasonal Campaign Playbook',
    content: `Maximize seasonal opportunities with targeted campaigns that drive consistent traffic throughout the year:

**🌻 Spring Campaign (March-May)**
• **Theme**: "Spring Into Savings" - Car maintenance and travel prep
• **Key Offers**: Oil change discounts, tire rotation specials, car wash packages
• **Travel Focus**: Spring break and Easter travel fuel deals
• **Timing**: Target tax refund season for larger purchases

**☀️ Summer Campaign (June-August)**
• **Theme**: "Summer Road Trip Ready" - Travel and convenience focus
• **Key Offers**: Travel snack bundles, cold beverage deals, ice cream promotions
• **Service Focus**: AC checks, coolant service, tire pressure monitoring
• **Peak Times**: Memorial Day, 4th of July, Labor Day weekend specials

**🍂 Fall Campaign (September-November)**
• **Theme**: "Back-to-School & Winter Prep" - Routine and preparation
• **Key Offers**: Morning coffee deals for school run parents, winterization services
• **Community Focus**: Support local school fundraisers and sports teams
• **Preparation**: Winter emergency kit sales, battery testing promotions

**❄️ Winter Campaign (December-February)**
• **Theme**: "Warm Up Your Savings" - Comfort and reliability focus
• **Key Offers**: Hot beverage promotions, loyalty program bonuses, fuel additives
• **Service Focus**: Battery testing, emergency roadside kit sales
• **Holiday**: Gift card promotions and loyalty point bonuses

**📱 Seasonal Communication Calendar:**
• **Email Campaigns**: Monthly seasonal newsletters with relevant offers
• **SMS Alerts**: Weather-triggered promotions (heat wave = car wash deals)
• **App Notifications**: Seasonal service reminders and maintenance alerts
• **Social Media**: Share seasonal tips and community engagement content

**🎯 Targeted Seasonal Audiences:**
• **Spring**: New homeowners, college students returning, tax refund recipients
• **Summer**: Families planning vacations, daily commuters, weekend travelers
• **Fall**: Parents with school-age children, hunters, winter prep shoppers
• **Winter**: Holiday shoppers, cold-weather commuters, winter sports enthusiasts

**📈 Seasonal Performance Tracking:**
• Compare year-over-year seasonal revenue growth
• Track seasonal offer redemption rates
• Monitor customer acquisition during peak seasons
• Analyze seasonal service upsell success rates

**🚀 Seasonal Success Tips:**
1. Plan campaigns 4-6 weeks in advance of seasonal peaks
2. Stock seasonal products and promote bundled offers
3. Train staff on seasonal upselling techniques and customer needs
4. Leverage weather data for timely promotional triggers`
  }
}

const methodologyItems = [
  {
    icon: '🎯',
    title: 'Local Targeting',
    description: 'Precision targeting of customers within your service area using location data and customer behavior patterns.'
  },
  {
    icon: '📊',
    title: 'Performance Analytics',
    description: 'Track campaign effectiveness, customer acquisition costs, and ROI for every promotional initiative.'
  },
  {
    icon: '🤝',
    title: 'Customer Engagement',
    description: 'Multi-channel approach using SMS, email, app notifications, and in-store promotions for maximum reach.'
  },
  {
    icon: '💡',
    title: 'Smart Recommendations',
    description: 'AI-powered suggestions for optimal timing, pricing, and promotional strategies based on local market data.'
  }
]

const sendMessage = async (message, promptId = null) => {
  if ((!message || !message.trim()) && !promptId) return
  
  const userMessage = message || (promptId ? prompts.find(p => p.id === promptId)?.description : '')
  if (!userMessage) return
  
  isLoading.value = true
  
  // Add user message
  messages.value.push({
    type: 'user',
    content: userMessage,
    timestamp: new Date().toLocaleTimeString()
  })
  
  // Clear input
  newMessage.value = ''
  
  // Simulate AI thinking time
  await new Promise(resolve => setTimeout(resolve, 1500))
  
  // Generate response based on prompt ID or analyze message content
  let responseData
  if (promptId && responses[promptId]) {
    responseData = responses[promptId]
  } else {
    // Try to match message content to a prompt category
    const matchedPrompt = prompts.find(prompt => 
      prompt.keywords.some(keyword => 
        userMessage.toLowerCase().includes(keyword.toLowerCase())
      )
    )
    
    if (matchedPrompt && responses[matchedPrompt.id]) {
      responseData = responses[matchedPrompt.id]
    } else {
      // Default response for franchise owners
      responseData = {
        title: '⛽ Shell Station Marketing Support',
        content: `I understand you're looking for marketing support for your Shell station. Here are some ways I can help:

**🎯 Campaign Types I Can Assist With:**
• **Local Promotions**: Coffee deals, fuel discounts, convenience store specials
• **Customer Acquisition**: New customer welcome campaigns and retention strategies  
• **Community Engagement**: Local events, partnerships, and community involvement
• **Seasonal Marketing**: Holiday promotions, weather-based campaigns, and seasonal offers

**📱 Available Channels Through MORE! Connected:**
• SMS marketing to your customer database
• Email newsletters and promotional campaigns
• Mobile app push notifications for nearby customers
• In-store promotional materials and displays

**💡 Quick Tip for Franchise Owners:**
Focus on local, relevant offers that solve immediate customer needs. Morning coffee deals work great for commuter traffic, while weekend car wash specials attract family customers.

**🚀 To Get Started:**
Choose one of the specific campaign types above, or tell me about a particular promotion you'd like to run at your station. I'll provide detailed implementation steps and messaging templates.

What type of campaign would you like to develop for your Shell station?`
      }
    }
  }
  
  // Add assistant response
  messages.value.push({
    type: 'assistant',
    content: responseData.content,
    title: responseData.title,
    timestamp: new Date().toLocaleTimeString()
  })
  
  isLoading.value = false
  
  // Auto-scroll to bottom
  await nextTick()
  const container = document.querySelector('.messages-container')
  if (container) {
    container.scrollTop = container.scrollHeight
  }
}

const handlePromptClick = (promptId) => {
  const prompt = prompts.find(p => p.id === promptId)
  if (prompt) {
    sendMessage(prompt.description, promptId)
  }
}

const formatMessage = (content) => {
  return content
    .replace(/\n/g, '<br>')
    .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
    .replace(/• /g, '<span class="bullet">•</span> ')
}
</script>

<template>
  <div class="chat-container">
    <!-- Header -->
    <header class="chat-header">
      <div class="header-content">
        <div class="shell-logo">
          <div class="shell-icon">
            <img src="/src/assets/shell_logo.png" alt="Shell Logo" class="shell-logo-img">
          </div>
          <div class="logo-text">
            <h1>Shell Energy Marketing Hub</h1>
            <span class="tagline">Campaign Development Assistant</span>
          </div>
        </div>
      </div>
    </header>

    <!-- Main Chat Area -->
    <main class="chat-main">
      <!-- Welcome Section with Predefined Prompts -->
      <div v-if="messages.length === 0" class="welcome-section">
        <div class="welcome-content">
          <div class="welcome-header">
            <div class="shell-energy-logo">
              <div>
                <h2>Shell Marketing Hub</h2>
                <p>Connected marketing platform for Shell franchise owners to promote local offers and engage customers in their community.</p>
              </div>
            </div>
          </div>
          
          <div class="prompt-grid">
            <button 
              v-for="prompt in prompts" 
              :key="prompt.title"
              @click="handlePromptClick(prompt.id)"
              class="prompt-card"
              :style="{'--accent-color': prompt.color}"
              :disabled="isLoading"
            >
              <div class="prompt-icon">{{ prompt.icon }}</div>
              <div class="prompt-content">
                <h3>{{ prompt.title }}</h3>
                <p>{{ prompt.description }}</p>
              </div>
              <div class="prompt-accent"></div>
            </button>
          </div>


        </div>
      </div>

      <!-- Messages Section -->
      <div v-else class="messages-container">
        <div v-for="message in messages" :key="message.timestamp" class="message" :class="message.type">
          <div class="message-content">
            <div class="message-avatar">
              <span v-if="message.type === 'user'">👤</span>
              <span v-else>⛽</span>
            </div>
            <div class="message-body">
              <div class="message-text" v-html="formatMessage(message.content)"></div>
              <div class="message-time">{{ message.timestamp }}</div>
            </div>
          </div>
        </div>
        
        <!-- Typing Indicator -->
        <div v-if="isLoading" class="message assistant">
          <div class="message-content">
            <div class="message-avatar">⛽</div>
            <div class="message-body">
              <div class="typing-indicator">
                <div class="typing-text">Analyzing your campaign needs...</div>
                <div class="typing-dots">
                  <div class="typing-dot"></div>
                  <div class="typing-dot"></div>
                  <div class="typing-dot"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>

    <!-- Chat Input -->
    <div class="chat-footer">
      <div class="input-container">
        <div class="input-wrapper">
          <input 
            v-model="newMessage"
            @keyup.enter="sendMessage(newMessage)"
            :disabled="isLoading"
            type="text" 
            placeholder="Describe your marketing challenge or promotion idea..."
            class="message-input"
          />
          <button 
            @click="sendMessage(newMessage)"
            :disabled="isLoading || !newMessage.trim()"
            class="send-button"
          >
            <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
              <path d="M2.01 21L23 12 2.01 3 2 10l15 2-15 2z"/>
            </svg>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.chat-container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  background: #f5f5f5;
  font-family: 'Shell Headline', -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
}

/* Header */
.chat-header {
  background: #1e1e1e;
  padding: 1rem 2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  border-bottom: none;
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
  flex-wrap: wrap;
  gap: 2rem;
}

.shell-logo {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.shell-icon {
  width: 6rem;
  height: 6rem;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.shell-icon svg {
  display: none;
}

.shell-logo-img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  position: relative;
  z-index: 10;
}

.logo-text h1 {
  margin: 0;
  color: white;
  font-size: 1.8rem;
  font-weight: 700;
  line-height: 1.2;
}

.tagline {
  font-size: 0.9rem;
  color: #cccccc;
  font-weight: 400;
}

.header-badges {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.badge {
  background: transparent;
  padding: 0.25rem 0.75rem;
  border-radius: 0;
  font-size: 0.85rem;
  font-weight: 400;
  color: white;
  border: none;
  border-bottom: 2px solid transparent;
  transition: border-color 0.3s ease;
}

.badge:hover {
  border-bottom-color: #FECB00;
}

/* Main Chat Area */
.chat-main {
  flex: 1;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  background: #f5f5f5;
}

/* Welcome Section */
.welcome-section {
  flex: 1;
  padding: 3rem 2rem;
  overflow-y: auto;
}

.welcome-content {
  max-width: 1200px;
  margin: 0 auto;
}

.welcome-header {
  text-align: center;
  margin-bottom: 3rem;
}

.shell-energy-logo {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 2rem;
}

.energy-icon {
  background: #FECB00;
  color: #1e1e1e;
  width: 4rem;
  height: 4rem;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  box-shadow: 0 4px 20px rgba(254, 203, 0, 0.3);
}

.welcome-content h2 {
  color: #1e1e1e;
  font-size: 2.5rem;
  margin: 0 0 1rem 0;
  font-weight: 700;
  line-height: 1.2;
}

.welcome-content p {
  color: #666;
  font-size: 1.1rem;
  line-height: 1.6;
  max-width: 700px;
  margin: 0 auto;
}

.prompt-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 2rem;
  margin-bottom: 4rem;
}

.prompt-card {
  background: white;
  border: none;
  border-radius: 16px;
  padding: 2rem;
  cursor: pointer;
  transition: all 0.3s ease;
  text-align: left;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
  position: relative;
  overflow: hidden;
}

.prompt-card::before {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: var(--accent-color);
  transform: scaleX(0);
  transition: transform 0.3s ease;
}

.prompt-card:hover::before {
  transform: scaleX(1);
}

.prompt-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
}

.prompt-card:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

.prompt-icon {
  font-size: 2.5rem;
}

.prompt-content h3 {
  margin: 0 0 0.5rem 0;
  color: #1e1e1e;
  font-size: 1.3rem;
  font-weight: 700;
}

.prompt-content p {
  margin: 0;
  color: #666;
  font-size: 0.95rem;
  line-height: 1.5;
}

.methodology-section {
  background: white;
  border-radius: 16px;
  padding: 3rem;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
  border: none;
}

.methodology-section h3 {
  text-align: center;
  color: #1e1e1e;
  font-size: 1.8rem;
  margin: 0 0 2rem 0;
  font-weight: 700;
}

.methodology-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
}

.method-item {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  padding: 1.5rem;
  background: #f8f9fa;
  border-radius: 12px;
  transition: all 0.3s ease;
}

.method-item:hover {
  background: #FECB00;
  transform: translateY(-2px);
}

.method-item:hover .method-text strong,
.method-item:hover .method-text span {
  color: #1e1e1e;
}

.method-icon {
  background: white;
  color: #1e1e1e;
  width: 3rem;
  height: 3rem;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  flex-shrink: 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.method-text {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.method-text strong {
  color: #1e1e1e;
  font-size: 1.1rem;
  font-weight: 700;
  transition: color 0.3s ease;
}

.method-text span {
  color: #666;
  font-size: 0.9rem;
  line-height: 1.4;
  transition: color 0.3s ease;
}

/* Messages */
.messages-container {
  flex: 1;
  overflow-y: auto;
  padding: 2rem;
  scroll-behavior: smooth;
  max-width: 1000px;
  margin: 0 auto;
  width: 100%;
}

.message {
  margin-bottom: 2rem;
}

.message-content {
  display: flex;
  gap: 1rem;
  align-items: flex-start;
}

.message.user .message-content {
  flex-direction: row-reverse;
}

.message-avatar {
  width: 3rem;
  height: 3rem;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  flex-shrink: 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.message.user .message-avatar {
  background: #1e1e1e;
  color: white;
}

.message.assistant .message-avatar {
  background: #FECB00;
  color: #1e1e1e;
}

.message-body {
  flex: 1;
  max-width: 75%;
}

.message.user .message-body {
  text-align: right;
}

.message-text {
  background: white;
  padding: 1.5rem 2rem;
  border-radius: 16px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
  line-height: 1.6;
  color: #1e1e1e;
  font-size: 1rem;
  border: none;
}

.message.user .message-text {
  background: #1e1e1e;
  color: white;
}

.message.assistant .message-text {
  border-left: 4px solid #FECB00;
}

.message-text :deep(.bullet) {
  color: #FECB00;
  font-weight: bold;
}

.message.user .message-text :deep(.bullet) {
  color: #FECB00;
}

.message.user .message-text :deep(strong) {
  color: #FECB00;
}

.message-time {
  font-size: 0.8rem;
  color: #999;
  margin-top: 0.5rem;
  font-weight: 400;
}

/* Typing Indicator */
.typing-indicator {
  background: white;
  padding: 1.5rem 2rem;
  border-radius: 16px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
  display: flex;
  flex-direction: column;
  gap: 1rem;
  border-left: 4px solid #FECB00;
}

.typing-text {
  color: #1e1e1e;
  font-weight: 600;
  font-size: 1rem;
}

.typing-dots {
  display: flex;
  gap: 0.5rem;
}

.typing-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #FECB00;
  animation: typing 1.4s infinite ease-in-out;
}

.typing-dot:nth-child(2) {
  animation-delay: 0.2s;
}

.typing-dot:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes typing {
  0%, 60%, 100% {
    transform: translateY(0);
    opacity: 0.5;
  }
  30% {
    transform: translateY(-6px);
    opacity: 1;
  }
}

/* Footer/Input */
.chat-footer {
  background: white;
  padding: 2rem;
  border-top: 1px solid #e0e0e0;
  box-shadow: 0 -4px 16px rgba(0, 0, 0, 0.05);
}

.input-container {
  max-width: 1000px;
  margin: 0 auto;
}

.input-wrapper {
  display: flex;
  gap: 1rem;
  align-items: center;
  background: #f8f9fa;
  padding: 1rem 1.5rem;
  border-radius: 8px;
  border: 2px solid #e0e0e0;
  transition: all 0.3s ease;
}

.input-wrapper:focus-within {
  border-color: #FECB00;
  box-shadow: 0 0 0 3px rgba(254, 203, 0, 0.1);
}

.message-input {
  flex: 1;
  border: none;
  background: transparent;
  font-size: 1rem;
  padding: 0.5rem 0;
  outline: none;
  color: #1e1e1e;
  font-weight: 400;
}

.message-input::placeholder {
  color: #999;
  font-weight: 400;
}

.send-button {
  background: #FECB00;
  color: #1e1e1e;
  border: none;
  width: 3rem;
  height: 3rem;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  font-weight: 600;
}

.send-button svg {
  width: 1.2rem;
  height: 1.2rem;
}

.send-button:hover:not(:disabled) {
  background: #f5c000;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(254, 203, 0, 0.3);
}

.send-button:disabled {
  background: #ccc;
  cursor: not-allowed;
  transform: none;
  color: #999;
}

.input-hint {
  text-align: center;
  margin-top: 1rem;
  color: #666;
  font-size: 0.85rem;
  font-weight: 400;
}

/* Shell-style Read More Links */
.read-more {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  color: #1e1e1e;
  text-decoration: none;
  font-weight: 600;
  margin-top: 1rem;
  transition: all 0.3s ease;
}

.read-more::after {
  content: '→';
  transition: transform 0.3s ease;
}

.read-more:hover {
  color: #FECB00;
}

.read-more:hover::after {
  transform: translateX(4px);
}

/* Responsive Design */
@media (max-width: 768px) {
  .chat-header {
    padding: 1rem;
  }

  .header-content {
    flex-direction: column;
    text-align: center;
    gap: 1rem;
  }

  .shell-logo {
    justify-content: center;
  }

  .logo-text h1 {
    font-size: 1.5rem;
  }

  .welcome-content h2 {
    font-size: 2rem;
  }

  .welcome-content p {
    font-size: 1rem;
  }

  .prompt-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .methodology-section {
    padding: 2rem;
  }

  .methodology-grid {
    grid-template-columns: 1fr;
  }

  .messages-container {
    padding: 1rem;
  }

  .message-body {
    max-width: 85%;
  }

  .chat-footer {
    padding: 1rem;
  }

  .header-badges {
    justify-content: center;
  }
}

@media (max-width: 480px) {
  .welcome-content h2 {
    font-size: 1.8rem;
  }

  .prompt-content h3 {
    font-size: 1.2rem;
  }

  .shell-icon {
    width: 2.5rem;
    height: 2.5rem;
  }

  .logo-text h1 {
    font-size: 1.3rem;
  }

  .energy-icon {
    width: 3rem;
    height: 3rem;
    font-size: 1.5rem;
  }

  .method-item {
    padding: 1rem;
  }
}

/* Shell Brand Enhancements */
.shell-gradient {
  background: linear-gradient(135deg, #FECB00 0%, #D52B1E 100%);
}

.shell-card-hover {
  transition: all 0.3s ease;
}

.shell-card-hover:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
}

/* Professional Shell Energy styling */
.energy-branding {
  position: relative;
}

.energy-branding::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, #FECB00 0%, #D52B1E 100%);
}
</style>

