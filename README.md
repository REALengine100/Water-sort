# Water Sort — Android প্রজেক্ট (Capacitor)

এই ফোল্ডারে গেম, আইকন, স্প্ল্যাশ ও APK বানানোর সব কনফিগ আছে।
Android Studio ছাড়াই **GitHub Actions** দিয়ে APK বানানো যায়। চাইলে নিজের কম্পিউটারেও বানানো যায়।

## পদ্ধতি ১: GitHub দিয়ে APK (Android Studio লাগবে না)
1. github.com-এ একটি নতুন repository খুলুন (Private হলেও চলবে)।
2. এই ফোল্ডারের সব ফাইল (`.github` ফোল্ডারসহ) repository-তে আপলোড করুন। টার্মিনালে:
   ```bash
   git init && git add . && git commit -m "Water Sort"
   git branch -M main
   git remote add origin <আপনার-repo-লিংক>
   git push -u origin main
   ```
3. repository-র **Actions** ট্যাবে "Build Android APK" চলবে (৫–১০ মিনিট)।
4. শেষ হলে ওই run খুলে নিচের **Artifacts** থেকে `water-sort-apk` ডাউনলোড করুন।
5. জিপ খুলে `app-debug.apk` ফোনে পাঠান। ফোনে "Unknown sources / এই উৎস থেকে ইনস্টল" অনুমতি দিয়ে ইনস্টল করুন।

## পদ্ধতি ২: নিজের কম্পিউটারে
দরকার: Node.js 22+, JDK 21, Android Studio (সর্বশেষ)।
```bash
npm install
npx cap add android
npx capacitor-assets generate --android
npx cap sync android
npx cap open android
```
Android Studio-তে *Build → Build APK(s)* দিয়ে APK বানান।
পোর্ট্রেট লক করতে `AndroidManifest.xml`-এর `<activity` ট্যাগে `android:screenOrientation="portrait"` যোগ করুন।

## Google Play-তে দিতে (AAB)
- Android Studio → *Build → Generate Signed Bundle / APK → Android App Bundle*।
- keystore ফাইল ও পাসওয়ার্ড নিরাপদ জায়গায় ব্যাকআপ রাখুন। হারালে অ্যাপ আপডেট করা যাবে না।
- `.aab` ফাইল Play Console-এ আপলোড করুন। Privacy Policy, স্ক্রিনশট ও Data safety ফর্ম লাগবে।
- debug APK শুধু নিজে বা বন্ধুদের টেস্টের জন্য। Play Store-এ রিলিজ APK/AAB দিতে হয়।

## কাস্টমাইজ
- **প্যাকেজ নাম:** `capacitor.config.json`-এর `appId` (Play Store-এ একবার দিলে আর বদলানো যায় না)। আগেই নিজের নাম দিন, যেমন `com.yourname.watersort`।
- **অ্যাপের নাম:** `appName`।
- **আইকন/স্প্ল্যাশ:** `assets/` ফোল্ডারের ছবি বদলান (icon-only 1024px, splash 2732px)।
- **গেম কোড:** `www/index.html`।

## নোট
- ফন্ট Google Fonts থেকে লোড হয়। অফলাইনে ফলব্যাক ফন্ট দেখাবে।
- Back বোতাম: ওপেন থাকা মেনু বন্ধ করে, নইলে হোমে যায়, হোমে থাকলে অ্যাপ বন্ধ হয়।
- অ্যাপ ব্যাকগ্রাউন্ডে গেলে সাউন্ড থামে। ভাইব্রেশনে Capacitor Haptics ব্যবহার হয়।
- Capacitor 8 এর জন্য Node 22+ ও JDK 21 দরকার। Play Store বর্তমানে API 36 টার্গেট চায়, Capacitor 8 তা মেনে চলে।
