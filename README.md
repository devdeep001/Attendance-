# Attendance-
Attendance project based on python code
DEFAULT_NAMES = [
    "Alice", "Bob", "Charlie", "David", "Eve",
    "Frank", "Grace", "Heidi", "Ivan", "Judy"
]

def get_yes_no(prompt, valid_yes=("p","present","y","yes"), valid_no=("a","absent","n","no")):
    while True:
        v = input(prompt).strip().lower()
        if v == "":
            continue
        if v in valid_yes:
            return True
        if v in valid_no:
            return False
        print("Please enter P/p (present) or A/a (absent).")

def get_int_in_range(prompt, lo=0, hi=100):
    while True:
        s = input(prompt).strip()
        try:
            x = int(s)
            if lo <= x <= hi:
                return x
            print(f"Enter an integer between {lo} and {hi}.")
        except ValueError:
            print("Enter a valid integer.")

def main():
    print("Attendance & Pass/Fail Register for 10 students")
    use_defaults = input("Use default student names? (Y/n): ").strip().lower()
    if use_defaults == "" or use_defaults.startswith("y"):
        names = DEFAULT_NAMES.copy()
    else:
        names = []
        print("Enter 10 student names (press Enter after each):")
        for i in range(10):
            name = input(f"Name {i+1}: ").strip()
            if name == "":
                name = f"Student{i+1}"
            names.append(name)

    pass_mark = input("Enter pass mark (default 40): ").strip()
    try:
        pass_mark = int(pass_mark)
    except Exception:
        pass_mark = 40

    records = []
    print("\nFor each student, enter P for present or A for absent.")
    for i, name in enumerate(names, start=1):
        present = get_yes_no(f"{i}. {name} - Present? (P/A): ")
        if not present:
            records.append({"name": name, "status": "Absent", "marks": None, "result": "Absent"})
            continue
        marks = get_int_in_range(f"Enter marks for {name} (0-100): ", 0, 100)
        result = "Pass" if marks >= pass_mark else "Fail"
        records.append({"name": name, "status": "Present", "marks": marks, "result": result})

    # Print report
    print("\nAttendance Report")
    print("-" * 60)
    print(f"{'No.':<4} {'Name':<15} {'Status':<8} {'Marks':<6} {'Result':<6}")
    print("-" * 60)
    pass_count = fail_count = absent_count = 0
    present_count = 0
    for idx, r in enumerate(records, start=1):
        marks_str = "-" if r["marks"] is None else str(r["marks"])
        print(f"{idx:<4} {r['name']:<15} {r['status']:<8} {marks_str:<6} {r['result']:<6}")
        if r["status"] == "Absent":
            absent_count += 1
        else:
            present_count += 1
            if r["result"] == "Pass":
                pass_count += 1
            else:
                fail_count += 1

    print("-" * 60)
    total = len(records)
    attendance_pct = (present_count / total) * 100
    pass_rate = (pass_count / present_count * 100) if present_count else 0.0

    print(f"Total students : {total}")
    print(f"Present        : {present_count}")
    print(f"Absent         : {absent_count}")
    print(f"Passed         : {pass_count}")
    print(f"Failed         : {fail_count}")
    print(f"Attendance %   : {attendance_pct:.1f}%")
    print(f"Pass rate among present students: {pass_rate:.1f}%")

if __name__ == "__main__":
    main()
