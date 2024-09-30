Custom Flows: Build a custom email or SMS OTP authentication flow



1.  [Custom Flows](/docs/custom-flows/overview)
2.  Email / SMS OTP

Build a custom email or SMS OTP authentication flow
===================================================

Warning

This guide is for users who want to build a _custom_ user interface using the Clerk API. To use a _prebuilt_ UI, use Clerk's [Account Portal pages](/docs/customization/account-portal/overview) or [prebuilt components](/docs/components/overview).

Clerk supports passwordless authentication, which lets users sign in and sign up without having to remember a password. Instead, users receive a one-time password (OTP), also known as a one-time code, via email or SMS, which they can use to authenticate themselves.

This guide will walk you through how to build a custom SMS OTP sign-up and sign-in flow. The process for using email OTP is similar, and the differences will be highlighted throughout.

### [Enable SMS OTP](#enable-sms-otp)

To use SMS OTP as an authentication strategy, you first need to enable it in the Clerk Dashboard.

1.  Navigate to the [Clerk Dashboard](https://dashboard.clerk.com/last-active?path=user-authentication/email-phone-username).
2.  In the navigation sidebar, go to **User & Authentication > Email, Phone, Username**.
3.  Ensure that _only_ **Phone number** is required. If **Email address** or **Username** are enabled, ensure they are not required. Use the settings cog icon to check if a setting is required or optional. If you would like to require **Username**, you must collect the username and pass it to the `create()` method in your custom flow. For this guide, if you would like to use email OTP instead, require the **Email address** option instead of the **Phone number** option.
4.  In the **Authentication strategies** section of this page, ensure **SMS verification code** is enabled. Ensure **Password** is toggled off, as you are prioritizing passwordless, SMS OTP-only authentication in this guide. If you would like to use email OTP instead, enable the **Email verification code** strategy instead of the **SMS verification code** strategy.

### [Sign-up flow](#sign-up-flow)

To sign up a user using an OTP, you must:

1.  Initiate the sign-up process by collecting the user's identifier, which for this example is a phone number.
2.  Prepare the verification, which sends a one-time code to the given identifier.
3.  Attempt to complete the verification with the code the user provides.
4.  If the verification is successful, set the newly created session as the active session.

Next.js

JavaScript

iOS (Beta)

This example is written for Next.js App Router but can be adapted to any React meta framework, such as Remix.

app/sign-up/\[\[...sign-up\]\]/page.tsx

    'use client'
    
    import * as React from 'react'
    import { useSignUp } from '@clerk/nextjs'
    import { useRouter } from 'next/navigation'
    
    export default function Page() {
      const { isLoaded, signUp, setActive } = useSignUp()
      const [verifying, setVerifying] = React.useState(false)
      const [phone, setPhone] = React.useState('')
      const [code, setCode] = React.useState('')
      const router = useRouter()
    
      async function handleSubmit(e: React.FormEvent) {
        e.preventDefault()
    
        if (!isLoaded && !signUp) return null
    
        try {
          // Start the sign-up process using the phone number method
          await signUp.create({
            phoneNumber: phone,
          })
    
          // Start the verification - a SMS message will be sent to the
          // number with a one-time code
          await signUp.preparePhoneNumberVerification()
    
          // Set verifying to true to display second form and capture the OTP code
          setVerifying(true)
        } catch (err) {
          // See https://clerk.com/docs/custom-flows/error-handling
          // for more info on error handling
          console.error('Error:', JSON.stringify(err, null, 2))
        }
      }
    
      async function handleVerification(e: React.FormEvent) {
        e.preventDefault()
    
        if (!isLoaded && !signUp) return null
    
        try {
          // Use the code provided by the user and attempt verification
          const signInAttempt = await signUp.attemptPhoneNumberVerification({
            code,
          })
    
          // If verification was completed, set the session to active
          // and redirect the user
          if (signInAttempt.status === 'complete') {
            await setActive({ session: signInAttempt.createdSessionId })
    
            router.push('/')
          } else {
            // If the status is not complete, check why. User may need to
            // complete further steps.
            console.error(signInAttempt)
          }
        } catch (err) {
          // See https://clerk.com/docs/custom-flows/error-handling
          // for more info on error handling
          console.error('Error:', JSON.stringify(err, null, 2))
        }
      }
    
      if (verifying) {
        return (
          <>
            <h1>Verify your phone number</h1>
            <form onSubmit={handleVerification}>
              <label htmlFor="code">Enter your verification code</label>
              <input value={code} id="code" name="code" onChange={(e) => setCode(e.target.value)} />
              <button type="submit">Verify</button>
            </form>
          </>
        )
      }
    
      return (
        <>
          <h1>Sign up</h1>
          <form onSubmit={handleSubmit}>
            <label htmlFor="phone">Enter phone number</label>
            <input
              value={phone}
              id="phone"
              name="phone"
              type="tel"
              onChange={(e) => setPhone(e.target.value)}
            />
            <button type="submit">Continue</button>
          </form>
        </>
      )
    }

Expand code

To create a sign-up flow for email OTP, use the [`prepareEmailAddressVerification`](/docs/references/javascript/sign-up/email-verification#prepare-email-address-verification) and [`attemptEmailAddressVerification`](/docs/references/javascript/sign-up/email-verification#attempt-email-address-verification). These helpers work the same way as their phone number counterparts do in the previous example. You can find all available methods in the [`SignUp`](/docs/references/javascript/sign-in/sign-in) object documentation.

### [Sign-in flow](#sign-in-flow)

To authenticate a user with an OTP, you must:

1.  Initiate the sign-in process by creating a `SignIn` using the identifier provided, which for this example is a phone number.
2.  Prepare the first factor verification.
3.  Attempt verification with the code the user provides.
4.  If the attempt is successful, set the newly created session as the active session.

Next.js

JavaScript

iOS (Beta)

This example is written for Next.js App Router but can be adapted to any React meta framework, such as Remix.

app/sign-in/\[\[...sign-in\]\]/page.tsx

    'use client'
    
    import * as React from 'react'
    import { useSignIn } from '@clerk/nextjs'
    import { PhoneCodeFactor, SignInFirstFactor } from '@clerk/types'
    import { useRouter } from 'next/navigation'
    
    export default function Page() {
      const { isLoaded, signIn, setActive } = useSignIn()
      const [verifying, setVerifying] = React.useState(false)
      const [phone, setPhone] = React.useState('')
      const [code, setCode] = React.useState('')
      const router = useRouter()
    
      async function handleSubmit(e: React.FormEvent) {
        e.preventDefault()
    
        if (!isLoaded && !signIn) return null
    
        try {
          // Start the sign-in process using the phone number method
          const { supportedFirstFactors } = await signIn.create({
            identifier: phone,
          })
    
          // Filter the returned array to find the 'phone_code' entry
          const isPhoneCodeFactor = (factor: SignInFirstFactor): factor is PhoneCodeFactor => {
            return factor.strategy === 'phone_code'
          }
          const phoneCodeFactor = supportedFirstFactors?.find(isPhoneCodeFactor)
    
          if (phoneCodeFactor) {
            // Grab the phoneNumberId
            const { phoneNumberId } = phoneCodeFactor
    
            // Send the OTP code to the user
            await signIn.prepareFirstFactor({
              strategy: 'phone_code',
              phoneNumberId,
            })
    
            // Set verifying to true to display second form
            // and capture the OTP code
            setVerifying(true)
          }
        } catch (err) {
          // See https://clerk.com/docs/custom-flows/error-handling
          // for more info on error handling
          console.error('Error:', JSON.stringify(err, null, 2))
        }
      }
    
      async function handleVerification(e: React.FormEvent) {
        e.preventDefault()
    
        if (!isLoaded && !signIn) return null
    
        try {
          // Use the code provided by the user and attempt verification
          const signInAttempt = await signIn.attemptFirstFactor({
            strategy: 'phone_code',
            code,
          })
    
          // If verification was completed, set the session to active
          // and redirect the user
          if (signInAttempt.status === 'complete') {
            await setActive({ session: signInAttempt.createdSessionId })
    
            router.push('/')
          } else {
            // If the status is not complete, check why. User may need to
            // complete further steps.
            console.error(signInAttempt)
          }
        } catch (err) {
          // See https://clerk.com/docs/custom-flows/error-handling
          // for more info on error handling
          console.error('Error:', JSON.stringify(err, null, 2))
        }
      }
    
      if (verifying) {
        return (
          <>
            <h1>Verify your phone number</h1>
            <form onSubmit={handleVerification}>
              <label htmlFor="code">Enter your verification code</label>
              <input value={code} id="code" name="code" onChange={(e) => setCode(e.target.value)} />
              <button type="submit">Verify</button>
            </form>
          </>
        )
      }
    
      return (
        <>
          <h1>Sign in</h1>
          <form onSubmit={handleSubmit}>
            <label htmlFor="phone">Enter phone number</label>
            <input
              value={phone}
              id="phone"
              name="phone"
              type="tel"
              onChange={(e) => setPhone(e.target.value)}
            />
            <button type="submit">Continue</button>
          </form>
        </>
      )
    }

Expand code

To create a sign-in flow for email OTP, pass the value `email_code` as the first factor strategy. You can find all available methods in the [`SignIn`](/docs/references/javascript/sign-in/sign-in) object documentation.

Feedback
--------

What did you think of this content?

It was helpfulIt was not helpfulI have feedback

[Edit this page on GitHub](https://github.com/clerk/clerk-docs/edit/main/docs/custom-flows/email-sms-otp.mdx)

Last updated on Sep 27, 2024

Support
