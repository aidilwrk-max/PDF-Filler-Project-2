<!DOCTYPE html>
<html>
<head>
    <title>PDF Generator</title>
    <!-- Load the pdf-lib library -->
    <script src="https://unpkg.com"></script>
    <style>
        body { font-family: Arial, sans-serif; margin: 30px; line-height: 1.6; background-color: #f4f4f4; }
        .container { max-width: 500px; background: white; padding: 20px; border-radius: 8px; box-shadow: 0px 0px 10px rgba(0,0,0,0.1); }
        .input-group { margin-bottom: 15px; }
        label { display: block; font-weight: bold; margin-bottom: 5px; }
        input { padding: 8px; width: 95%; border: 1px solid #ccc; border-radius: 4px; }
        button { padding: 10px 15px; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; width: 100%; font-size: 16px; }
        button:hover { background: #0056b3; }
    </style>
</head>
<body>

<div class="container">
    <h2>Fill Info to Generate PDF</h2>
    
    <div class="input-group">
        <label>Full Name:</label>
        <input type="text" id="userName" placeholder="asd">
    </div>
    
    <div class="input-group">
        <label>Date:</label>
        <input type="text" id="currentDate" placeholder="2026-06-24">
    </div>

    <button onclick="generatePDF()">Generate & Download PDF</button>
</div>

    <script>
        async function generatePDF() {
            const nameValue = document.getElementById('userName').value;
            const dateValue = document.getElementById('currentDate').value;

            if (!nameValue || !dateValue) {
                alert('Please fill out all fields first!');
                return;
            }

            try {
                // Changing this line to find your renamed 'template.pdf.pdf'
                const existingPdfBytes = await fetch('./template.pdf.pdf').then(res => {
                    if(!res.ok) throw new Error('PDF file not found');
                    return res.arrayBuffer();
                });

                const pdfDoc = await PDFLib.PDFDocument.load(existingPdfBytes);
                const pages = pdfDoc.getPages();
                const firstPage = pages[0];

                // Write text onto your template
                firstPage.drawText(nameValue, { x: 150, y: 500, size: 14 });
                firstPage.drawText(dateValue, { x: 150, y: 450, size: 14 });

                const pdfBytes = await pdfDoc.save();
                const blob = new Blob([pdfBytes], { type: "application/pdf" });
                const link = document.createElement('a');
                link.href = window.URL.createObjectURL(blob);
                link.download = "Filled_Form.pdf";
                link.click();

            } catch (error) {
                console.error(error);
                alert('Error generating PDF. Make sure "template.pdf.pdf" is uploaded to your main repository folder.');
            }
        }
    </script>

</body>
</html>
