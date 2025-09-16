Sources:

[](https://www.youtube.com/watch?app=desktop&v=RhpZkmE-lOs)

[OWASP Cambridge & BCS Cybercrime SG “Adding SAST to CI/CD, Without Losing Any Friends” - Tanya Janca](https://www.youtube.com/watch?app=desktop&v=RhpZkmE-lOs)

### First gen SAST

- very slow
    
- finds everything
    
- lots of false positives
    
- costs a lot to fix / triage all the issues.
    

### Next gen SAST

- Symbolic execution
    
    - source - sink analysis
        
        - e.g. source = API input
            
        - sink = where the input data is used
            
        - validation is required in between
            
    - Free version of semgrep doesn’t do this across files, paid version does this
        
    - lots of paid SAST doesn’t do this
        
- Anti-pattern matching
    
    - Finds XSS etc. through pattern matching (use of eval)
        
- Very fast
    
- Mostly true positives
    
- Typically created after the pandemic, made for devops
    

### Selecting a SAST tool

- does it support my programming languages?
    
- does it provide more than just what the free open source tool provides?
    
    - test it!
        
- Does it work with all my CI tools? My IDEs?
    
- Will I have to share access to my source code?
    
- Do you need custom rules? Often most valuable signals!
    

### Custom rules

- If you get a pentest result or bug bounty submission: create a custom rule to find other places where (the same) developer(s) made the same mistake.
    

### SAST in IDE

- push left, integrate in IDE (red squiggly lines while coding)
    
- check on every checkin
    
- Sanity check in CI
    

SAST in CI

- roll out high signal checks
    
- Find friendly dev teams to test
    
- Staged roll-out start with notify → then PR comments → then block build