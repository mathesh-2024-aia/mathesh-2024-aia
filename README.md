import streamlit as st
import random

#session state
if 'game_stage' not in st.session_state:
    st.session_state.game_stage ='user_guessing'
    st.session_state.target_number = random.randint(1, 50)
    st.session_state.attempts = 0
    st.session_state.machine_low = 1
    st.session_state.machine_high = 50
    st.session_state.machine_guess = random.randint(st.session_state.machine_low,st.session_state.machine_high)
    
#title of the game
    st.title("Guess the number")
    
#user's turn
if st.session_state.game_stage == 'user_guessing':
    st.header ("User turn to guess")
    st.write("The number lies between 1 to 50")
#user input    
    user_guess = st.number_input("Enter your guess :",min_value=1,max_value=50, step=1)
#submit button
    if st.button("Submit guess"):
        st.session_state.attempts +=1
        if user_guess < st.session_state.target_number:
            st.write("Too low! Try again.")
        elif user_guess > st.session_state.target_number:
            st.write("Too high! Try again.")
        else:
            st.write(f"Congrats! You guessed it in {st.session_state.attempts}attempts!")
            st.write(f"Now it's computer's turn to guess your number.") 
            
#computer's turn
            st.session_state.game_stage = 'computer_guessing'

#to restart reseting the attempts for computer's turn
            st.session_state.attempts = 0

#computer's turn
elif st.session_state.game_stage == 'computer_guessing':
    st.header("Computer's Turn to guess")
    st.write("Think a number between 1 to 50")
    st.write(f"My guess is: {st.session_state.machine_guess}")
     
#feedback for computer's guessing
    feedback = st.radio("Is the guess too low, too high or correct?", ('Too low','Too high','Correct'))
     
#submit button
    if st.button("Submit feedback"):
        st.session_state.attempts +=1
        if feedback == 'Too low':
            st.session_state.machine_low = st.session_state.machine_guess +1
        elif feedback == 'Too high':
            st.session_state.machine_high = st.session_state.machine_guess -1
        elif feedback == 'Correct':
            st.write(f"Hurrah!! I guessed your number correctly in {st.session_state.attempts}attempts")
#to restart the game
            st.write("Starting a new game.")
            st.session_state.game_stage = 'user_guessing'
            st.session_state.target_number = random.randint(1,50)

            st.session_state.attempts = 0
            st.session_state.machine_low = 1
            st.session_state.machine_high = 50

            st.session_state.machine_guess = random.randint(st.session_state.machine_low,st.session_state.machine_high)
            st.stop()

        if feedback != 'Correct':
            if st.session_state.machine_low <= st.session_state.machine_high:
                st.session_state.machine_guess = random.randint(st.session_state.machine_low,st.session_state.machine_high)
            else:
                st.write("Oops! Something went wrong. Let's restart the game.")
                st.session_state.game_stage = 'user_guessing'


