# Ass-2
# Ass-2
class Room:
    def __init__(self, room_number: int, room_type: str, amenities: list, price_per_night: float):
        """
        Create a room with its number, type, amenities list, and price.
        Sets availability to True by default.
        """
        self.__room_number = room_number
        self.__room_type = room_type
        self.__amenities = amenities
        self.__price_per_night = price_per_night
        self._is_available = True


    # Getter and Setter Methods


    def get_room_number(self):
        """Return the room number."""
        return self.__room_number


    def set_room_number(self, number):
        """Set a new room number."""
        self.__room_number = number


    def get_room_type(self):
        """Return the room type (e.g., single, double)."""
        return self.__room_type


    def set_room_type(self, room_type):
        """Set a new room type."""
        self.__room_type = room_type


    def get_amenities(self):
        """Return a list of amenities in the room."""
        return self.__amenities


    def set_amenities(self, amenities):
        """Set a new list of amenities."""
        self.__amenities = amenities


    def get_price_per_night(self):
        """Return the price per night."""
        return self.__price_per_night


    def set_price_per_night(self, price):
        """Set a new price per night."""
        self.__price_per_night = price


    def is_available(self):
        """Check if the room is available."""
        return self._is_available


    def set_availability(self, status):
        """Set room availability to True or False."""
        self._is_available = status


    def get_room_info(self):
        """Return a description of the room."""
        return f"Room {self.__room_number} ({self.__room_type}), Price: ${self.__price_per_night}/night"


    def __str__(self):
        return self.get_room_info()


class Guest:
    def __init__(self, name: str, email: str, phone: str, loyalty_status: str = "Regular"):
        """
        Create a guest profile with personal details and an empty booking history.
        """
        self.__name = name
        self.__email = email
        self.__phone = phone
        self._loyalty_status = loyalty_status
        self.__reservation_history = []


    # Getter and Setter Methods


    def get_name(self):
        """Return the guest's name."""
        return self.__name


    def set_name(self, name):
        """Set a new name."""
        self.__name = name


    def get_email(self):
        """Return the guest's email."""
        return self.__email


    def set_email(self, email):
        """Set a new email address."""
        self.__email = email


    def get_phone(self):
        """Return the guest's phone number."""
        return self.__phone


    def set_phone(self, phone):
        """Set a new phone number."""
        self.__phone = phone


    def get_loyalty_status(self):
        """Return the loyalty status."""
        return self._loyalty_status


    def set_loyalty_status(self, status):
        """Set a new loyalty status."""
        self._loyalty_status = status


    def get_reservation_history(self):
        """Return the list of past bookings."""
        return self.__reservation_history


    def add_to_history(self, booking):
        """Add a booking to the guest's reservation history."""
        self.__reservation_history.append(booking)


    def create_account(self):
        """Simulate account creation message."""
        return f"Account created for {self.__name}"


    def update_profile(self, name=None, email=None, phone=None):
        """Update guest profile information if new values are given."""
        if name:
            self.set_name(name)
        if email:
            self.set_email(email)
        if phone:
            self.set_phone(phone)


    def view_history(self):
        """Print all past bookings."""
        return [str(booking) for booking in self.__reservation_history]


    def __str__(self):
        return f"Guest: {self.__name}, Email: {self.__email}, Phone: {self.__phone}"


class Booking:
    def __init__(self, booking_id: str, guest, room, check_in: str, check_out: str):
        """
        Create a booking with a guest, room, and check-in/check-out dates.
        Adds the booking to the guest’s history and marks the room as unavailable.
        """
        self.__booking_id = booking_id
        self.__guest = guest
        self.__room = room
        self.__check_in = check_in
        self.__check_out = check_out
        self.__invoice = None
        guest.add_to_history(self)
        room.set_availability(False)


    def calculate_total(self):
        """Calculate the total price for the stay (1 night by default)."""
        return self.__room.get_price_per_night() * 1


    def confirm_booking(self):
        """Return a confirmation message for the booking."""
        return f"Booking {self.__booking_id} confirmed for {self.__guest} in {self.__room}"


    def cancel_booking(self):
        """Cancel the booking and set the room back to available."""
        self.__room.set_availability(True)


    def set_invoice(self, invoice):
        """Link an invoice to this booking."""
        self.__invoice = invoice


    def __str__(self):
        return self.confirm_booking()


class Invoice:
    def __init__(self, booking, extra_charges: float = 0.0, discount: float = 0.0):
        """
        Create an invoice based on a booking.
        Includes room price, extra charges, discounts, and calculates the total.
        """
        self.__booking = booking
        self.__room_price = booking.calculate_total()
        self.__extra_charges = extra_charges
        self.__discount = discount
        self.__total_amount = self.__room_price + extra_charges - discount
        self._payment_status = "Unpaid"


    def generate_invoice(self):
        """Return a summary of the invoice with the total amount."""
        return f"Invoice for {self.__booking}: Total = ${self.__total_amount}"


    def apply_discount(self):
        """Apply the discount and update the total."""
        self.__total_amount -= self.__discount


    def process_payment(self):
        """Mark the invoice as paid."""
        self._payment_status = "Paid"


    def __str__(self):
        return self.generate_invoice()


class LoyaltyProgram:
    def __init__(self, guest):
        """
        Create a loyalty program account for a guest.
        Keeps track of earned and redeemed points.
        """
        self.__guest = guest
        self.__points = 0


    def add_points(self, nights: int):
        """Add 10 points per night stayed."""
        self.__points += nights * 10


    def redeem_points(self, amount: int):
        """Use points if the guest has enough."""
        if self.__points >= amount:
            self.__points -= amount


    def get_points(self):
        """Return how many points the guest has."""
        return self.__points


    def __str__(self):
        return f"{self.__guest} has {self.__points} loyalty points"


class ServiceRequest:
    def __init__(self, guest, request_type: str):
        """
        Create a request (like housekeeping or room service) made by a guest.
        """
        self.__guest = guest
        self.__request_type = request_type
        self.__status = "Pending"


    def make_request(self):
        """Describe what the guest requested."""
        return f"{self.__guest} requested {self.__request_type}"


    def update_status(self, new_status: str):
        """Update the request status (e.g., Completed or In Progress)."""
        self.__status = new_status


    def __str__(self):
        return f"Request: {self.__request_type}, Status: {self.__status} for {self.__guest}"


class Feedback:
    def __init__(self, guest, comment: str, rating: int):
        """
        Allow a guest to leave feedback with a comment and rating out of 5.
        """
        self.__guest = guest
        self.__comment = comment
        self.__rating = rating


    def __str__(self):
        return f"Feedback from {self.__guest}: '{self.__comment}' ({self.__rating}/5)"
