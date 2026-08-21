from pathlib import Path0
import os


class DiskAnalyzer:
    def __init__(self, root):
        self.root = Path(root)

    def get_folder_size(self, folder):
        total = 0

        try:
            for path, dirs, files in os.walk(folder):
                for file in files:
                    try:
                        file_path = Path(path) / file
                        total += file_path.stat().st_size
                    except:
                        pass
        except:
            pass

        return total

    def format_size(self, size):
        for unit in ["B", "KB", "MB", "GB", "TB"]:
            if size < 1024:
                return f"{size:.2f} {unit}"
            size /= 1024

    def analyze(self):
        results = []

        print("Scanning folders...\n")

        for item in self.root.iterdir():

            if item.is_dir():

                size = self.get_folder_size(item)

                results.append((item.name, size))

        results.sort(key=lambda x: x[1], reverse=True)

        print("=" * 60)
        print(f"{'Folder':35} {'Size'}")
        print("=" * 60)

        for folder, size in results[:20]:
            print(
                f"{folder:35} {self.format_size(size)}"
            )


if __name__ == "__main__":

    path = input(
        "Enter directory path: "
    ).strip()

    analyzer = DiskAnalyzer(path)

    analyzer.analyze()
