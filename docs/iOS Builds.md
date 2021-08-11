# Building for iOS

Twine App Builder will generate you an iOS app and help you distribute it. 

Be warned that this setup is notably more complicated than any of the other targets Twine App Builder supports, and also requires you to have a paid iOS Developer account with Apple ($100 USD/year). It does _not_ require you to use a Mac.

Creating an iOS version of your app through Twine App Builder is slightly different than other platforms. Instead of generating a binary you can download and then distribute however you want, Twine App Builder handles uploading your game directly to Apple. You'll have the option to either submit a build to the App Store or for TestFlight beta distribution (see "Distribution" below). Since Apple only allows you to upload new builds a certain number of times per day, you'll have to manually run the correspnding GitHub Action workflow instead of it happening automatically every time you change your game.

## Apple Developer Account
Setup instructions. Note real-name issues.

