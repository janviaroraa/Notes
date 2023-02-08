# Notes

                      |------------------ CGAffineTransformComponents ----------------|
 
       | a  b  0 |     | sx  0  0 |   |  1  0  0 |   | cos(t)  sin(t)  0 |   | 1  0  0 |
       | c  d  0 |  =  |  0 sy  0 | * | sh  1  0 | * |-sin(t)  cos(t)  0 | * | 0  1  0 |
       | tx ty 1 |     |  0  0  1 |   |  0  0  1 |   |   0       0     1 |   | tx ty 1 |
   CGAffineTransform      scale           shear            rotation          translation
 
# Error Types

Swift Error Handling Strategies:
 1. Result
 2. Throw
 3. Assert
 4. Precondition
 5. FatalError
 
 Categories of Swift Errors:
 1. Logical
    The result of programmer mistake; e.g. index out of bounds.
 2. Run-time
    Outside of programmer control; e.g. file not found.
 
 ??
 Levels of Errors:
        KIND                   RECOVERY
      Optional<T>         Deal with nil. Skip.
       Result<T>          Deals with an error. Skip.
        throw             catch to deal with an error. Skip with try?. Assert no  error with try!.
       assert()           Fallback for production.
    precondition()        None
      fatalError()        None
 
    Going from least critical to most critical.
 
 1. Result:
    a. represents either a success with a value or a failure with an error.
    b. provides more context on what caused the failure
 
 2. Throw:         (do-try-catch block)
    a. propagates an Error-conforming type
    b. Throwing an error indicates that something unexpected has happened and the normal flow of execution can’t continue.
    c. It automatically passes control to the first appropriate catch clause.
 
 3. Assert
    a. Assertion tests for conditions that should never happen if your code is correct.
    b. It is active only in development mode and is skipped in production.
    c. Assertions allow to track down false assumptions during development, so that they can be fixed early. Using any other mechanism from the previous levels (throw, Optional or Result) for this purpose will be a mistake, since they represent a condition that the program has to recover from at runtime.
 
 4. Precondition:
    a. Precondition works identically to assertion, except it terminates the app both in development and production, if its condition evaluates to be false.
 
 MARK: - Assert vs Precondition
 Use assert() to check your code for internal errors.
 Use precondition() when consuming arguments from your clients. These parameters require public documentation.
 
 5. fatalError:
    a. terminates your app unconditionally.
    b. returns Never
    c. When considering between fatalError() and precondition(), take into account that the former adds failure message to the crash report, while the latter doesn’t.

 MARK: - Imp
 recoverable: Optional, Result, throw;
 and non-recoverable: assert(), precondition(), fatalError().

  
  # NSCoder
  NSCoder is related to Interface Builder (IB) -> in case of storyboards.
     Everything that we set and do in storyboards, Xcode will then translate it to code behind then scenes. The way it translates this code is by decoding it, using NSCoder.
     But if we are not using storyboards, we will need to tell Xcode "the required init method". This won't cause our app to crash because there ain't any storyboards to decode.
     
     super.init(coder: coder) can also be used instead of fatalError
  
  
  # fatalError
  fatalError: can be used in 2 ways, i.e. <br>
     1. fatalError() <br>
     2. fatalError("with a message") 
  
        The message that is passed to the function is printed to the console along with the file and line on which the fatalError(_:file:line:) function is called. And your application immediately stops execution.
     
        E.g : fatal error: with a message: file /Task-Team-UI/SubtaskCell.swift, line 20
     
        DECLARATION:
         func fatalError(
             _ message: @autoclosure () -> String = String(),
             file: StaticString = #file,
             line: UInt = #line
         ) -> Never
     
         EXITING THE ABOVE FUNCTION:
         enum Never
         The return type of functions that do not return normally, i.e. a type with no values.
     
     Cannot catch a fatal error in a do-catch statement.
     
     Downside : The application is terminated without any form of warning or precondition validation.
     
     Only use fatalError if the application enters an unknown state, a state it doesn't know how to handle. Remember that the application will crash and burn if it throws a fatal error.
