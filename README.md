//ass1-1]audio

import java.util.Scanner;
interface MediaPlayer {
    void play(String audioType, String fileName);}
interface AdvancedMediaPlayer {
    void playMp4(String fileName);
    void playVlc(String fileName);}
class Mp4Player implements AdvancedMediaPlayer {
    public void playMp4(String fileName) {
        System.out.println("Playing MP4 file. Name: " + fileName); }
public void playVlc(String fileName) {}}
class VlcPlayer implements AdvancedMediaPlayer {
public void playMp4(String fileName) {    }
public void playVlc(String fileName) {
        System.out.println("Playing VLC file. Name: " + fileName);}}
class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer advancedMediaPlayer;
  public MediaAdapter(String audioType) {
        if (audioType.equalsIgnoreCase("mp4")) {
            advancedMediaPlayer = new Mp4Player();
        } else if (audioType.equalsIgnoreCase("vlc")) {
            advancedMediaPlayer = new VlcPlayer();  } }
  public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("mp4")) {
            advancedMediaPlayer.playMp4(fileName);
        } else if (audioType.equalsIgnoreCase("vlc")) {
            advancedMediaPlayer.playVlc(fileName);} }}
class AudioPlayer implements MediaPlayer {
    private MediaAdapter mediaAdapter;
 public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("mp3")) {
            System.out.println("Playing MP3 file. Name: " + fileName);
        } else if (audioType.equalsIgnoreCase("mp4") || audioType.equalsIgnoreCase("vlc")) {
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType, fileName);
        } else {
            System.out.println("Invalid media type: " + audioType + ". Supported formats are MP3, MP4, VLC.");}}}
public class AdapterPattern {
    public static void main(String[] args) {
        AudioPlayer audioPlayer = new AudioPlayer();
        Scanner scanner = new Scanner(System.in);
System.out.println("Enter the file type (mp3, mp4, vlc):");
        String audioType = scanner.nextLine();
  System.out.println("Enter the file name:");
        String fileName = scanner.nextLine();
  audioPlayer.play(audioType, fileName);
        scanner.close();}}
