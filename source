void main() {
	namedWindow(window_name, 1);
	VideoCapture capture("1.avi");
	Mat frame, frame_gray, edges, gradientX, gradientY, cropped;
	vector<Vec4i> lines;
	int frame_counter = 1;
	int minLineLength = 30;
	int maxLineGap = 10;
	int LposX = 0, LposY = 0, RposX = 0, RposY = 0;
	capture >> frame;
	int frameH = frame.size().height;
	int frameW = frame.size().width;
	int cropH = 3 * frameH / 4;
	int cropW = frameW;
	int cf = 0;
	while (cf++<3)
	{
		capture >> frame;

		// This line picks out the rectangle from the image
		// and copies to a new Mat
		frame(Rect(0, cropH, cropW, frameH / 4)).copyTo(cropped);

		//cvtColor(frame, frame_gray, COLOR_BGR2GRAY);
		Canny(cropped, edges, 50, 350);
		
		//threshold(frame, edges, 128, 255, THRESH_BINARY_INV);
		HoughLinesP(edges, lines, 1, 3.14 / 180, 20, minLineLength, maxLineGap);

		int countL = 0, countR = 0;
		vector<int> aLposX, aLposY, aRposX, aRposY;
		for (int i = 0; i < lines.size(); i++)
		{
			Vec4i l = lines[i];
			
			if (!(abs(l[0] - l[2]) >= 0 && abs(l[0] - l[2]) <= 3) && !(abs(l[1] - l[3]) >= 0 && abs(l[1] - l[3]) <= 5)) {
				if (abs(l[0] + l[2]) < frameW-20) {
					aLposX.push_back(abs(l[0] + l[2]));
					aLposY.push_back(abs(l[1] + l[3]));
					countL++;
				}
				else {
					aRposX.push_back(abs(l[0] + l[2]));
					aRposY.push_back(abs(l[1] + l[3]));
					countR++;
				}
				
				line(cropped, Point(l[0], l[1]), Point(l[2], l[3]), Scalar(0, 255, 0), 2);
			}
		}

		for (int i = 0; i < countL; i++)
		{
			LposX += aLposX[i];
			LposY += aLposY[i];
			cout << aLposX[i] << endl;
		}

		for (int i = 0; i < countR; i++)
		{
			RposX += aRposX[i];
			RposY += aRposY[i];
		}
		cout << countL << " " << countR << endl;
		LposX /= countL * 2;
		LposY /= countL * 2;
		RposX /= countR * 2;
		RposY /= countR * 2;
		cout << (LposX + RposX) / 2 << " " << (LposY + RposY) / 2 << endl;
		line(cropped, Point((LposX + RposX) / 2, (LposY + RposY) / 2), Point((LposX + RposX) / 2, (LposY + RposY) / 2), Scalar(0, 0, 255), 2);
		//Sobel(frame, gradientX, CV_32F, 1, 0, 1);
		//Sobel(frame, gradientY, CV_32F, 0, 1, 1);
		//addWeighted(gradientX, 0.5, gradientX, 0.5, 0, frame);


		frame_counter += 1;
		if(frame_counter == capture.get(CV_CAP_PROP_FRAME_COUNT)){
			frame_counter = 0;
			capture.set(CV_CAP_PROP_POS_FRAMES, 0);
		}
		imshow(window_name, cropped);
		waitKey(33);
	}
	system("pause");
}
