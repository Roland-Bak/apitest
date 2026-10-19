// Minimal Vision OCR check: draws "HELLO WORLD" and runs Accurate text recognition.
// Run with: swift ocr-check.swift
import AppKit
import Vision

// 1. Render a synthetic 1100x240 image with black text on white.
let w = 1100, h = 240
let rep = NSBitmapImageRep(bitmapDataPlanes: nil, pixelsWide: w, pixelsHigh: h,
                           bitsPerSample: 8, samplesPerPixel: 4, hasAlpha: true,
                           isPlanar: false, colorSpaceName: .deviceRGB,
                           bytesPerRow: 0, bitsPerPixel: 0)!
NSGraphicsContext.saveGraphicsState()
NSGraphicsContext.current = NSGraphicsContext(bitmapImageRep: rep)
NSColor.white.setFill()
NSRect(x: 0, y: 0, width: w, height: h).fill()
("HELLO WORLD" as NSString).draw(at: NSPoint(x: 60, y: 70), withAttributes: [
    .font: NSFont.systemFont(ofSize: 96, weight: .bold),
    .foregroundColor: NSColor.black,
])
NSGraphicsContext.restoreGraphicsState()
guard let cgImage = rep.cgImage else { print("ERROR: could not create CGImage"); exit(3) }

// 2. Run Accurate OCR with the same settings the customer used.
let request = VNRecognizeTextRequest()
request.recognitionLevel = .accurate
request.recognitionLanguages = ["en-US"]
request.usesLanguageCorrection = false

var text = ""
do {
    try VNImageRequestHandler(cgImage: cgImage, options: [:]).perform([request])
} catch {
    print("ERROR: \(error)")
}

// 3. Report what came back.
let results = request.results ?? []
print("observations: \(results.count)")
for obs in results {
    if let best = obs.topCandidates(1).first {
        print("\(best.string) conf=\(best.confidence)")
        text += best.string + " "
    }
}

let ok = text.uppercased().contains("HELLO") && text.uppercased().contains("WORLD")
print(ok ? "RESULT: PASS" : "RESULT: FAIL")
exit(ok ? 0 : 1)
