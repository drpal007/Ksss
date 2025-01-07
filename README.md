# Ksss
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KssS Hotel - Online Room Booking</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f5f5f5;
            color: #333;
        }

        /* Header */
        header {
            background-color: #2c3e50;
            color: #ffffff;
            text-align: center;
            padding: 20px;
        }

        h1, h2 {
            margin: 10px 0;
        }

        /* Form Section */
        section {
            margin: 20px auto;
            max-width: 1200px;
            padding: 20px;
            text-align: center;
            background-color: #ffffff;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
        }

        section h2 {
            color: #2c3e50;
        }

        .form-group {
            margin: 10px auto;
            max-width: 400px;
        }

        .form-group input {
            width: 100%;
            padding: 10px;
            margin-top: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
            font-size: 16px;
        }

        .form-submit {
            background-color: #27ae60;
            color: #ffffff;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            margin-top: 15px;
        }

        .form-submit:hover {
            background-color: #1e8749;
        }

        /* Footer */
        footer {
            text-align: center;
            padding: 10px 0;
            background-color: #2c3e50;
            color: #ffffff;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <!-- Header -->
    <header>
        <h1>Welcome to KssS Hotel</h1>
        <p>Address: Almaspur Chowk, Kukra Road, Muzaffarnagar, 251001</p>
        <p>Phone: 9045989889</p>
    </header>

    <!-- Booking Form Section -->
    <section>
        <h2>Online Room Booking</h2>
        <p>Fill out the form below to book your room online.</p>
        <form id="bookingForm" onsubmit="verifyOTP(event)">
            <div class="form-group">
                <input type="text" id="customerName" placeholder="Enter Your Name" required>
            </div>
            <div class="form-group">
                <input type="date" id="bookingDate" required>
            </div>
            <div class="form-group">
                <input type="tel" id="customerMobile" placeholder="Enter Your Mobile Number" required pattern="[0-9]{10}" title="Enter a valid 10-digit mobile number">
            </div>
            <div class="form-group" id="otpSection" style="display: none;">
                <input type="text" id="otp" placeholder="Enter OTP" required>
            </div>
            <button type="submit" class="form-submit" id="submitButton">Send OTP</button>
        </form>
    </section>

    <!-- Footer -->
    <footer>
        &copy; 2024 KssS Hotel. All rights reserved.
    </footer>

    <!-- EmailJS Integration -->
    <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
    <script>
        (function() {
            // Initialize EmailJS with Public Key
            emailjs.init("R3lT0lj05hEW8rg_A");
        })();

        let otpSent = false; // Track OTP status
        const generatedOTP = Math.floor(1000 + Math.random() * 9000); // Simulated OTP

        // Function to Handle OTP Verification and Booking Submission
        function verifyOTP(event) {
            event.preventDefault();

            const name = document.getElementById('customerName').value;
            const date = document.getElementById('bookingDate').value;
            const mobile = document.getElementById('customerMobile').value;
            const otp = document.getElementById('otp').value;

            if (!otpSent) {
                // Simulate OTP sending
                alert(`Simulated OTP sent to ${mobile}: ${generatedOTP}`);
                otpSent = true;
                document.getElementById('otpSection').style.display = 'block';
                document.getElementById('submitButton').innerText = 'Confirm OTP';
            } else {
                // OTP Validation
                if (otp == generatedOTP) {
                    alert('OTP verified successfully! Booking is being submitted.');

                    // Proceed to Submit Booking via EmailJS
                    const templateParams = {
                        user_name: name,
                        booking_date: date,
                        user_mobile: mobile,
                    };

                    emailjs.send('service_33520o5', 'template_qf4mnsc', templateParams)
                        .then(function(response) {
                            alert('Booking Successful! We look forward to welcoming you.');
                            console.log('SUCCESS!', response.status, response.text);
                            document.getElementById('bookingForm').reset(); // Reset the form
                            otpSent = false; // Reset OTP state
                            document.getElementById('otpSection').style.display = 'none'; // Hide OTP Section
                            document.getElementById('submitButton').innerText = 'Send OTP'; // Reset button text
                        }, function(error) {
                            alert('Failed to Submit Booking. Please Try Again.');
                            console.log('FAILED...', error);
                        });
                } else {
                    alert('Invalid OTP. Please try again.');
                }
            }
        }
    </script>
</body>
</html>
