### F.I.R.S.T. rules

Clean tests should follow the rules:

-   **Fast** tests should be fast because we want to run them frequently.
    
-   **Independent** tests should not depend on each other. They should provide same output whether run independently or all together in any order.
    
-   **Repeatable** tests should be repeatable in any environment and there should be no excuse for why they fail.
    
-   **Self-Validating** a test should answer with either _Passed_ or _Failed_. You don't need to compare log files to answer if a test passed.
    
-   **Timely** unit tests should be written before the production code. If you write tests after the production code, you might find writing tests too hard.
    
