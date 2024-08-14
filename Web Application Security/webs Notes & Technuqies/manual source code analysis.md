


With this in mind, there are many high-priority items to consider when performing manual source
code analysis. This high-level list is presented in no particular order:
• After checking unauthenticated areas, focus on areas of the application that are likely to receive
less attention (i.e. authenticated portions of the application).
• Investigate how sanitization of the user input is performed. Is it done using a trusted, open-
source library, or is a custom solution in place?
• If the application uses a database, how are queries constructed? Does the application
parameterize input or simply sanitize it?
• Inspect the logic for account creation or password reset/recovery routines. Can the functionality
be subverted?
• Does the application interact with its operating system? If so, can we modify commands or
inject new ones?
• Are there programming language-specific vulnerabilities?
