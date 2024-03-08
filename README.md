# Custom Subscription Button for OneSignal Web SDK v16+.
Uses the code below to check subscriptions

```javascript
// Get the button
const osButton = document.querySelector(".onesignal-prompt-button");

// Function to update button text based on subscription status
const updateButtonText = () => {
  osButton.innerHTML = OneSignal.User.PushSubscription.optedIn
  ? "Unsubscribe from Push Notifications"
  : "Subscribe to Push Notifications";
};

// Set text for button on page load and subscription changes
OneSignalDeferred.push((OneSignal) => {
  updateButtonText();
  OneSignal.User.PushSubscription.addEventListener("change", updateButtonText);
});

// Prompt or Opt In / Opt Out, on button click
osButton.addEventListener("click", () => {
  const { User, Notifications } = OneSignal;
  
  if (User.PushSubscription.token) {
    User.PushSubscription.optedIn ? User.PushSubscription.optOut() : User.PushSubscription.optIn();
  } else {
    Notifications.requestPermission();
  }
});
