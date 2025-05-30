# Workout
A built in double-user workout tracker which tracks progress, predicts 1REP maxes and implements various data structures

# Creates the log to store users and lifts
workout_logs = {}

users = []

# Function which shows the user's lifts having been added
def show_lifts():
    if not users:
        print("No users found. Please add a user first.")
        return

    for user in users:
        print(f"\n{user}'s Lifts:")
        if not workout_logs[user]:
            print("  No workouts logged!")
        else:
            previous_max = {}
            for i, lift in enumerate(workout_logs[user], 1):
                lift_type = lift["type"]
                current_max = lift["max"]
                percent_change = ""

                if lift_type in previous_max:
                    old_max = previous_max[lift_type]
                    improvement = ((current_max - old_max) / old_max) * 100
                    sign = "+" if improvement >= 0 else ""
                    percent_change = f" ({sign}{improvement:.1f}%)"

                print(f"  {i}. {lift_type} — Max: {current_max:.1f} lbs{percent_change}")
                previous_max[lift_type] = current_max

# Function to add lifts to the logs
def add_lifts():
    if not users:
        print("No users found. Please add a user first.")
        return

    print("Available users:")
    for i, user in enumerate(users, 1):
        print(f"  {i}. {user}")
    user_index = int(input("Choose a user number: ")) - 1

    if user_index not in range(len(users)):
        print("Invalid user selected.")
        return

    user = users[user_index]
    lift_type = input("Squat, Bench, or Deadlift? ").capitalize()
    weight = int(input("Enter the weight (lbs): "))
    reps = int(input("Number of reps: "))
    est_max = weight / (1.0278 - 0.0278*reps)
    print(f"Max lift estimation: {est_max:.2f} lbs")

    workout_logs[user].append({"type": lift_type, "max": est_max})

# Function to add users to store workouts
def add_user():
    if len(users) >= 2:
        print("Maximum of 2 users reached.")
        return

    if not users:
        user_name = input("Enter first user's name: ")
        print(f"First user added as {user_name}")
    else:
        user_name = input("Enter second user's name: ")
        print(f"Second user added as {user_name}")

    users.append(user_name)
    workout_logs[user_name] = []

# Main loop to run the workout tracker
while True:
    print("\nOptions:")
    print("   1. View lifts")
    print("   2. Add lift")
    print("   3. Quit")
    print("   4. Add user")

    try:
        choice = int(input("Choose 1-4: "))
        if choice == 1:
            show_lifts()
        elif choice == 2:
            add_lifts()
        elif choice == 3:
            break
        elif choice == 4:
            add_user()
        else:
            print("Need to input valid number 1-4")
    except ValueError:
        print("Please enter a number.")
