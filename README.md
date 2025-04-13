# PLP-Python-assignment-week-7
import os

def file_transformer():
    """
    Reads a source file, applies transformations, and writes to a destination file.
    Handles various file-related errors gracefully.
    """
    print("=== File Transformer ===")
    print("This program will read a file, modify its content,")
    print("and save the transformed version to a new file.\n")
    
    while True:
        try:
            # Get file paths from user
            source_path = input("Enter path of the file to read: ").strip()
            dest_path = input("Enter path for the new file: ").strip()
            
            # Validate paths
            if not source_path:
                raise ValueError("Source file path cannot be empty")
            if not dest_path:
                raise ValueError("Destination file path cannot be empty")
            if os.path.abspath(source_path) == os.path.abspath(dest_path):
                raise ValueError("Source and destination cannot be the same file")
            if os.path.exists(dest_path):
                overwrite = input(f"'{dest_path}' already exists. Overwrite? (y/n): ").lower()
                if overwrite != 'y':
                    print("Operation cancelled.")
                    continue
            
            # Read and process file
            with open(source_path, 'r', encoding='utf-8') as src_file:
                original_content = src_file.read()
                modified_content = transform_content(original_content)
            
            # Write to new file
            with open(dest_path, 'w', encoding='utf-8') as dest_file:
                dest_file.write(modified_content)
            
            print(f"\nSuccess! Modified content written to {dest_path}")
            print(f"Original size: {len(original_content)} bytes")
            print(f"New size: {len(modified_content)} bytes")
            break
            
        except FileNotFoundError:
            print(f"\nError: File '{source_path}' not found. Please try again.")
        except PermissionError:
            print("\nError: Insufficient permissions to read/write files.")
        except UnicodeDecodeError:
            print("\nError: Could not decode the file (try a different encoding).")
        except ValueError as ve:
            print(f"\nError: {ve}")
        except Exception as e:
            print(f"\nAn unexpected error occurred: {type(e).__name__} - {str(e)}")
        
        # Offer retry option
        retry = input("\nWould you like to try again? (y/n): ").lower()
        if retry != 'y':
            print("Operation cancelled.")
            break

def transform_content(content):
    """
    Applies transformations to file content.
    Customize this function for different modifications.
    """
    # Example transformations:
    # 1. Capitalize each line
    # 2. Remove extra whitespace
    # 3. Add line numbers
    lines = content.splitlines()
    transformed_lines = []
    
    for i, line in enumerate(lines, 1):
        # Remove leading/trailing whitespace
        clean_line = line.strip()
        # Capitalize first letter
        if clean_line:
            transformed_line = f"{i}. {clean_line[0].upper()}{clean_line[1:]}"
        else:
            transformed_line = f"{i}. "
        transformed_lines.append(transformed_line)
    
    return '\n'.join(transformed_lines)

if __name__ == "__main__":
    file_transformer()
